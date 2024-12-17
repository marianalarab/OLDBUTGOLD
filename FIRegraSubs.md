#Regra de substituição - FI

Transação OBBH

![image](https://github.com/user-attachments/assets/9cf3f3d5-180e-482e-935c-f36bbd10b012)

![image](https://github.com/user-attachments/assets/86d6069d-94ee-4550-83c7-aeb1aba81cf5)

Você tem que uma regra com as condições, aqui temos esse exemplo, onde  FI_GL_TEXTO_H0001_TP e FI_GL_TEXTO_H0001_TRANS são SETS.
Que são criados na transação GS01

![image](https://github.com/user-attachments/assets/46832dc5-6059-455d-b530-20d31a245341)

A regra de substituição possui uma EXIT.

Programa com as regras de subst. ZGGBS000

O programa possui um form (get_exit_titles) no qual os forms das exits são configurados:

OBS: BKPF para cabeçalho e BSEG para item

```abap

  exits-name  = 'FIT02'.
  exits-param = c_exit_param_none.
  exits-title = text-117.             "Sum is used for the reference.
  APPEND exits.

*---------------------------------------------------------------------*
*       FORM u108                                                     *
*---------------------------------------------------------------------*
* Data: 10/03/2011                                                    *
* Objetivo: Substituição documentos de pagamento                     *
*---------------------------------------------------------------------*
FORM fit02.

  DATA: v_bln TYPE belnr_d,
        v_xblnr TYPE xblnr1.

  CLEAR: v_xblnr, v_bln.

  GET PARAMETER ID 'BLN' FIELD v_bln.

  IF sy-subrc EQ 0.
    SELECT SINGLE xblnr
      FROM bkpf
      INTO v_xblnr
      WHERE belnr EQ v_bln AND
      bukrs EQ bkpf-bukrs AND
      gjahr EQ bkpf-gjahr.

    IF sy-subrc EQ 0.
      MOVE: v_xblnr TO bkpf-xblnr.
    ENDIF.
  ENDIF.
ENDFORM.                    "tpmov
```




