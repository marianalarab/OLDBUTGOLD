# Executar um processo em outra Task

Criar uma RFC, ela será responsável por executar a task.

![image](https://github.com/user-attachments/assets/f75d61fd-edd0-4f0f-acee-704a92078a4f)

![image](https://github.com/user-attachments/assets/b5cab618-7326-4385-bea9-2f728da37046)

![image](https://github.com/user-attachments/assets/0fc689ff-d9e3-4da5-8fed-50e877a28947)

![image](https://github.com/user-attachments/assets/15eca19a-77c1-4f6e-bb78-af3a77e3edfe)

No Programa Chamar a função desta maneira:

```abap
w_taskname = 'TESTE'.
v_matnr    = '000000000000010161'.

CALL FUNCTION 'Z_MY_FUNCTION'
    STARTING NEW TASK w_taskname
    PERFORMING collect_data ON END OF TASK
    EXPORTING
        matnr = v_matnr
    TABLES 
        mat = t_mara.

```
O Taskname é o nome que ele dado para a task, se for mandar executar mais de uma task é necessário nomear diferente este parâmetro.

Em Seguida é importante colocar a condição. No caso:

```abap
WAIT UNTIL t_mara[] IS NOT INITIAL.
```

Este Comando wait é que chama o perform Collect_data, e aguarda enquando o Z_MY_FUNCTION não recebe valores de T_MARA.

```abap
FORM collect_data USING w_taskname.

    RECEIVE RESULTS FROM FUNCTION 'Z_MY_FUNCTION' 
    TABLES 
        mat = t_mara.
    
ENDFORM. "collect_data
```
#Codigo Fonte

```abap
REPORT ztestem.

DATA: v_max  TYPE i,
      v_free TYPE i.

DATA: w_taskname,
      v_matnr TYPE mara-matnr,
      t_mara TYPE TABLE OF mara,
      wa_mara LIKE LINE OF t_mara.

w_taskname = 'TESTE'.
v_matnr    = '000000000000010161'.

CALL FUNCTION 'Z_MY_FUNCTION'
    STARTING NEW TASK w_taskname
    PERFORMING collect_data ON END OF TASK
    EXPORTING
        matnr = v_matnr
    TABLES
        mat = t_mara.
        
WAIT UNTIL t_mara[] IS NOT INITIAL.

LOOP AT t_mara INTO wa_mara.
    WRITE: / wa_mara-matnr,
           / wa_mara-ersda,
           / wa_mara-ernam,
           / wa_mara-laeda,
           / wa_mara-aenam,
           / wa_mara-vpsta.
ENDLOOP.

*&---------------------------------------------------------------------*
*&  Form collect_data
*&---------------------------------------------------------------------*
*   text
*----------------------------------------------------------------------*
FORM collect_data USING w_taskname.

    RECEIVE RESULTS FROM FUNCTION 'Z_MY_FUNCTION'
    TABLES
        mat = t_mara. 
ENDFORM. "collect_data
```
