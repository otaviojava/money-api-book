### Métodos de criação:


Para criar uma instância de ```MonetaryAmount```, todas as implementações seguem o mesmo padrão de nomenclatura utilizando, com uma pequena exceção no ```RoundedMoney``` uma vez que ele pode receber um ```MonetaryOperator``` para trabalhar como “arredondador” em cada operação. Listando os mais importantes temos o total de três métodos, que são:

* O método “of” passando um number e um código de moeda.
* O método “zero” passando um CurrencyUnit.
* O método “ofMinor” passando um long e uma moeda, esse long será tratado como centavos e será convertido levando em consideração a moeda, por exemplo, 200 cents equivalem a dois dólares.
