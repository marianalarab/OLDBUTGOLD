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


```
