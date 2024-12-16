# Cast de objeto em ABAP - Exemplo com SAP CRM Action definition

CRMC_ACTION_DEF --> Transação para Action da CRM_ORDER. Para outras ver caminho na SPRO.

![image](https://github.com/user-attachments/assets/b71f071b-9016-408c-bd6f-2ac574e63df3)

Esta foi criada com o Action Profile ZBR6
Dois cliques em ZBR6

![image](https://github.com/user-attachments/assets/f87dad76-b269-486f-ae63-d8b281a153d5)

Em Action Definition da pra ver as definições para as Ações

![image](https://github.com/user-attachments/assets/3c215a0c-25e7-4db3-b7b7-1690da36b50e)

Em Processing Type a configuração, exibe o método que irá ser chamado.

![image](https://github.com/user-attachments/assets/24651e0b-c732-413f-ae52-82ada3d8fce9)

A Action vai chamar um método Z que deve chamar a RFC.
Esse método deve estender de EXEC_METHODCALL_PPF
Se19 -- EXEC_METHODCALL_PPF --> Criar
Nome da implementação criada
ZCRM_SEND_PO_VIA_RFC
Método: ZCRM_CALL_RFC
Utilizar a inteface Execute.

![image](https://github.com/user-attachments/assets/87269670-69db-4fed-8083-a6923d10cb52)

![image](https://github.com/user-attachments/assets/5b182f2a-9df3-4a3b-aef5-d0cf9535dac8)

Visualizar os parâmetros pela SE18

![image](https://github.com/user-attachments/assets/f1eb484e-fd4e-4c92-91dd-0dedbccc39c9)

Veja que o parâmetro IO_APPL_OBJECT é um OBJECT. Para podermos acessar o conteúdo temos que fazer um Cast desse objeto.
“Cast Object to application object”
O Cast é feito da seguinte forma:

```abap
DATA: lv_appl_object TYPE REF TO cl_doc_crm_order,
      ls_guid        TYPE crmt_object_guid,
      
*** Faz o Cast do Objeto.
    lv_appl_object ?= io_appl_object.
    
*** Obtém o GUID  
CALL METHOD lv_appl_object->get_crm_obj_guid
    RECEIVING
        result = ls_guid.

```

É declarado um objeto do tipo cl_doc_crm_order que possui o método get_crm_obj_guid. 
O comando ?= Faz o novo objeto receber o anterior e o método retorna o guid em uma variável.

![image](https://github.com/user-attachments/assets/4139b7ad-4d4e-4414-bb59-29b63fa3d136)



