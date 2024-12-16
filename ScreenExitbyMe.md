## Screen exit da transação ME22N, ME21N e ME23N

Ir na Transação e descobrir qual o nome do programa e a tela

![image](https://github.com/user-attachments/assets/153e4916-31bb-4fc2-ad45-261833cd87ae)

com o nome da ampliação ir na transação CMOD e criar um projeto.

![image](https://github.com/user-attachments/assets/9ac7deea-8f5a-4d58-a02e-e167c62f1773)

A tela é a 0101 como visto anteriormente.
Dois cliques no nome do programa.

```abap
PROCESS BEFORE OUTPUT.
  MODULE zm_status_0101.

PROCESS AFTER INPUT.
  MODULE zm_user_command_0101.

PROCESS ON VALUE-REQUEST.
  FIELD ekko_ci-zarea_sol
    MODULE zm_help_objid.
  FIELD ekko_ci-ztipo_contr
    MODULE zm_help_ztipo.
```

O Process on value request é para o Match Code dos campos

Ir em Layout e desenhar a tela como um Module Pool.

![image](https://github.com/user-attachments/assets/43b9f37e-d716-41e7-bf6d-e9d0bc7b2078)

Os Módulos PBO e PAI devem tratar os campos como se segue
PBO
No PBO deve básicamente fechar os campos para input quando a transação for ME23N(exibição) trazendo o valor do campo, fazendo um select na tabela do banco usando como chave o num do pedido. Para a transação ME22N o campo deve ser tratado igual a ME23N apenas o campo que deve estar aberto para edição já que é uma transação de alteração.

```abap
*----------------------------------------------------------------------*
***INCLUDE ZXM06O02 .
*----------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*&      Module  STATUS_0101  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE status_0101 OUTPUT.
  PERFORM ZIMM001.
ENDMODULE.                 " STATUS_0101  OUTPUT

FIELD-SYMBOLS:  <fs_postp>, 
                <fs_area>, 
                <fs_contr>, 
                <fs_retorno> TYPE ddshretval.

TYPES: BEGIN OF ekko_type,
    zarea_sol   TYPE ekko-zarea_sol, 
    ztipo_contr TYPE ekko-ztipo_contr,
END OF ekko_type.

DATA: gs_ekko TYPE ekko_type.

IF gc_trtyp EQ 'A' .
    LOOP AT SCREEN.
        screen-input = '0'.
        MODIFY SCREEN.
    ENDLOOP.
ENDIF.

ASSIGN: ('(SAPLMEGUI)MEPO_TOPLINE-EBELN') TO <fs_postp>. 

IF (sy-tcode EQ 'ME23N' OR sy-tcode EQ 'ME22N' OR sy-tcode EQ 'ME21N') 
    AND NOT <fs_postp> IS INITIAL 
    AND ( ekko_ci-zarea_sol IS INITIAL AND ekko_ci-ztipo_contr IS INITIAL).

    SELECT SINGLE zarea_sol ztipo_contr 
    INTO gs_ekko FROM ekko WHERE ebeln EQ <fs_postp>.
    
    MOVE: gs_ekko-zarea_sol TO ekko_ci-zarea_sol, gs_ekko-ztipo_contr TO ekko_ci-ztipo_contr. 
ENDIF.


```
PAI
Faz tratamento para os campos criados, exigido especialmente neste projeto.

```abap
*----------------------------------------------------------------------*
***INCLUDE ZXM06I04 .
*----------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*&      Module  ZUSER_COMMAND_0101  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE zuser_command_0101 INPUT.
  PERFORM zcampos_novos.
ENDMODULE.                 " ZUSER_COMMAND_0101  INPUT

FORM  zf_campos_novos.    

    TYPES:  BEGIN OF area_type, 
            zarea_sol TYPE  ekko-zarea_sol, 
            END OF area_type.
            
    DATA:  gw_area TYPE TABLE OF area_type.  
            
    IF sy-ucomm EQ 'MESAVE' AND (sy-tcode EQ 'ME21N' OR sy-tcode EQ 'ME22N' OR sy-tcode EQ 'ME23N'). 
    
        FIELD-SYMBOLS:  <fs_bsart>, <fs_area>, <fs_contr>, <fs_postp>.
        
        ASSIGN:  ('(SAPLMEGUI)MEPO_TOPLINE-BSART')  TO  <fs_bsart>,       
                 ('(SAPLXM06)EKKO_CI-ZAREA_SOL')    TO  <fs_area>,       
                 ('(SAPLXM06)EKKO_CI-ZTIPO_CONTR')  TO  <fs_contr>,     
                 ('(SAPLMEGUI)MEPO_TOPLINE-EBELN')  TO  <fs_postp>. 
    
        SELECT  objid FROM  plogi INTO  TABLE  gw_area WHERE objid EQ <fs_area>. 
        
        IF  sy-subrc  NE  0.
            MESSAGE  e022(zlmm01).  "Área  Solicitante  não  existente        
        ENDIF. 
        
        IF <fs_bsart> EQ 'ZNB' AND 
           <fs_area> IS  INITIAL AND      
           <fs_contr>  IS  INITIAL.     
           
            MESSAGE  i013(zlmm01) display like 'E'.
        
            CALL  SCREEN  0101. 
        
        ENDIF.
    ENDIF.
ENDFORM.                                        "  ZCAMPOS_NOVOS

```

MATCH CODE!
Observar bem o campo ztipo_contr. Pois o campo área de contratação possui esta função especial para MC. Já o Tipo de contr não, está usando uma função padrão.

PARA O CAMPO ZAREA_SOL:

```abap
*&---------------------------------------------------------------------*
*&   Module ZM_HELP_OBJID INPUT
*&---------------------------------------------------------------------*
*Ajuda de pesquisa para o campo Área de Colicitação
*----------------------------------------------------------------------*
MODULE zm_help_objid INPUT.
    DATA: progname TYPE d020s-prog,
          dynnum.  TYPE d020s-dnum,
          wa_field TYPE dynpread,
          t_fields TYPE TABLE OF dynpread.
          
    progname = sy-repid.
    dynnum   = sy-dynnr.
    
    CLEAR: t_fields, wa_field.
    wa_field-fieldname = 'EKKO-ZAREA_SOL'.
    APPEND wa_field TO t_fields.
    
    CALL FUNCTION 'DYNP_VALUES_READ'   
        EXPORTING    
            dyname               = progname
            dynumb               = dynnum
            translate_to_upper   = 'X'   
        TABLES
            dynpfields           = t_fields   
        EXCEPTIONS   
            invalid_abapworkarea = 1    
            invalid_dynprofield  = 2   
            invalid_dynproname   = 3
            invalid_dynpronummer = 4
            invalid_request      = 5
            no_fielddescription  = 6
            invalid_parameter    = 7
            undefind_error       = 8   
            double_conversion    = 9   
            stepl_not_found      = 10    
            OTHERS               = 11.


    DATA: v_otype TYPE objec-otype,
          v_text  TYPE hrp1000-stext.
          
    READ TABLE t_fields INTO wa_field INDEX 1. 
        v_otype = 'O'.
        
    CALL FUNCTION 'RH_DETERMINE_ORG_OBJID'   
        EXPORTING org_object_type   = v_otype
                  act_plvar         = '01'
                  act_search_string = '*'   
                  f4_mode           = 'X'   
        IMPORTING
                org_object_objid    = ekko_ci-zarea_sol   
        EXCEPTIONS
                no_active_plvar       = 1    
                no_object_id_selected = 2
                OTHERS                = 3.
ENDMODULE.   " ZM_HELP_OBJID  INPUT
```
