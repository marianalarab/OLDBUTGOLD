#Objeto de Bloqueio by Me

SE11 - EZPCEC

![image](https://github.com/user-attachments/assets/342caaae-9538-475d-bcdb-d6650d32d9a9)

Bloqueio da tabela ZPCEC

![image](https://github.com/user-attachments/assets/6b70b147-b834-4cad-b919-31b7d32e4412)

Chaves que utilizarão o bloqueio

![image](https://github.com/user-attachments/assets/321b30e6-014f-43b9-9010-19a846799b58)

Ex: Vai bloquear apenas se mais de um usuário quiser utilizar os mesmos dados desdes campos.
Se for outra empresa então não deve bloquear.

Ver o nome das funções em "Ir para - Módulos de Bloqueio"

![image](https://github.com/user-attachments/assets/1f79108c-171d-4674-b0d7-57dda24cc748)

Para bloquear:

```abap
CALL FUNCTION 'ENQUEUE_EZPCEC'
  EXPORTING
    mode_zpcec     = 'E'
    mandt          = sy-mandt
    zbukr          = p_bukrs
    hbkid          = p_hbkid
    hktid          = p_hktid
    x_zbukr        = ' '
    x_hbkid        = ' '
    x_hktid        = ' '
    _scope         = '2'
    _wait          = ' '
    _collect       = ' '
   EXCEPTIONS
    foreign_lock   = 1
    system_failure = 2
    OTHERS         = 3.

 MOVE: sy-subrc TO p_lock.
```

Se sy-subrc for igual a 0 então o bloqueio ocorreu com sucesso.

Para desbloquear:

```abap
CALL FUNCTION 'DEQUEUE_EZPCEC'
  EXPORTING
    mode_zpcec = 'E'
    mandt      = sy-mandt
    zbukr      = p_bukrs
    hbkid      = p_hbkid
    hktid      = p_hktid
    x_zbukr    = ' '
    x_hbkid    = ' '
    x_hktid    = ' '
    _scope     = '3'
    _synchron  = ' '
    _collect   = ' '.
```

[SAP OFFICIAL HELP](https://help.sap.com/docs/SAP_NETWEAVER_700/12a2d87e6c531014bec0e63ea0208c21/cf21eea5446011d189700000e8322d00.html?version=7.0.37)
