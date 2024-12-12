# Criando uma classe com herança de uma classe abstrata.

Minha idéia inicial era usar o método ROUND_VALUE da classe, mas como ela é abstrata eu não posso instanciar objetos para chamar a classe. Então criei uma classe filha com um método que chamaria esse ROUND_VALUE.

Classe:

![2024-12-12_15-57-24](https://github.com/user-attachments/assets/92c5e19a-b1b9-48ad-be82-7a18a935e4de)

Uma classe é abstrata quando possui métodos abstratos.
Os métodos abstratos têm abstract na sua descrição.

![image](https://github.com/user-attachments/assets/c90e9b3b-84cc-4729-8853-6c600df4c5b9)

Criando uma nova classe filha:
SE24
OBS: A SAP começou a recomendar o uso do Eclipse no lugar da transação SE24 (para desenvolvimento e manutenção de classes ABAP) com a introdução do ABAP Development Tools (ADT) para Eclipse, que foi disponibilizado a partir do SAP NetWeaver 7.40 (lançado em 2013).

![image](https://github.com/user-attachments/assets/b030f150-57b1-4490-9cb0-b44bf7c3bf56)

Publico: para eu poder instanciar um objeto no programa depois
Final: Não poderá ter herdeiros.

Clicar em ![image](https://github.com/user-attachments/assets/a2fdec68-5a4a-4714-a2cd-4c9ef407b630) para criar herança

![image](https://github.com/user-attachments/assets/7b7a5d3f-4c2c-4ede-8d2c-a93c5278f78b)

A nova classe vai possuir todos os métodos da classe pai.

![image](https://github.com/user-attachments/assets/3ea37d04-757d-42fd-9d4a-8a0a93089d91)

As classes abstratas devem ser REDEFINIDAS. Selecionar a classe abstrata e clicar em ![image](https://github.com/user-attachments/assets/c70ec59b-2d61-48c8-8f23-bf3f5dd5a957) Redefinir.

![image](https://github.com/user-attachments/assets/4163aaaa-7079-4656-88ad-09ae26f90bb9)

Não é preciso codificar nada. Apenas criar e redefinir.

![image](https://github.com/user-attachments/assets/7f3387c5-f64a-43f6-9d36-5f1f5232496b)

Fazer isso para todas os metodos abstratos da classe.
Claro que se identificar a nessecidade de implementar uma classe, esse trecho pode ser codificado.
Por fim, como era a minha intenção eu criei o meu método! E esse poderá ser chamado. Já que também é publico.

![image](https://github.com/user-attachments/assets/b07d72d3-ddda-46c3-aa41-9037d941f9b5)

Parâmetros:

![image](https://github.com/user-attachments/assets/9c97ea3c-0b87-4bf3-93b2-8869a783b2c0)

Código:

![image](https://github.com/user-attachments/assets/d65ea0c2-f3f6-40d9-ac10-f23cd7d9fb2f)

Instância do objeto e chamada no programa Z:
``` abap
DATA: zarredonda TYPE REF TO zcl_tax_calc_br_filha.

CREATE OBJECT zarredonda TYPE zcl_tax_calc_br_filha.

CALL METHOD zarredonda->zarredonda_kwert
	EXPORTING p_waerk = komk-waerk
	CHANGING zkwert = xkwert.

```














