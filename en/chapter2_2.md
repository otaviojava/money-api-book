### Representando dinheiro e moeda com o money-api



Na money-api, será usado esse nome em vez de JSR 354, também será necessário representar tanto o valor numérico quanto a moeda. Para representar a moeda existe a interface ```CurrencyUnit``` ela precisa ser imutável e thread-safe.

|Nome do método| Descrição |Exemplo|
| -- | -- | -- |
|```String getCurrencyCode()```|Retorna o código da moeda, para as moedas que seguem o ISO isso serão retornados três letras.|BRL para o real brasileiro, USD para dólares americanos.
|```int getNumericCode()```|Retorna o código numérico da moeda, assim como o código ele possui três dígitos.|986 para a moeda brasileira, 840 para dólares americanos.|
|``` int getDefaultFractionDigits()``` |Retorna o número de dígitos normalmente utilizado pela moeda.|BRL tem dois e JPY não tem.|


Nesse material a implementação utilizada será o **moneta**, a implementação de referência para essa especificação. Com ele é possível criar uma instância de moedas de duas formas. A primeira delas é utilizando o código da moeda, caso seja uma moeda que siga o padrão de moeda será uma ```String``` com três letras.


```java
public class CurrencyExample1 {

    public static void main(String[] args) {

        CurrencyUnit currencyUnit = Monetary.getCurrency("BRL");
        String currencyCode = currencyUnit.getCurrencyCode();//BRL
        int numericCurrencyCode = currencyUnit.getNumericCode();//986
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2

    }
}
```

A outra forma simples é utilizando a classe ```Locale```, essa criação é muito interessante, por exemplo, em uma aplicação web de compras em que a partir do ```Locale``` do request será possível saber qual é a moeda do usuário que está acessando a aplicação.

```java

public class CurrencyExample2 {

    public static void main(String[] args) {
        CurrencyUnit currencyUnit = Monetary.getCurrency(Locale.US);
        String currencyCode = currencyUnit.getCurrencyCode();//USD
        int numericCurrencyCode = currencyUnit.getNumericCode();//840
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2
    }
}

```

Definido a representação da moeda o próximo passo será a representação do valor monetário, para isso, existe a interface ```MonetaryAmount```, uma característica importante é que todas as implementações precisam ser imutável e thread-safe. 

|Método| Descrição|
| -- | -- |
|```<R> R query(MonetaryQuery<R> query)```|Realiza query com o valor monetário.|
|```MonetaryAmount with(MonetaryOperator operator)```|Realiza operações com o montante monetário.|
|```boolean isGreaterThan(MonetaryAmount amount)```|Retorna verdadeiro se a instância é maior que o valor passado no parâmetro, caso sejam iguais ele retornará falso.|
|```boolean   isGreaterThanOrEqualTo(MonetaryAmount amount)```|Retorna verdadeiro se a instância é maior ou igual que o valor passado no parâmetro, caso sejam iguais ele retornará verdadeiro.|
|```boolean isLessThan(MonetaryAmount amount)```|Retorna verdadeiro se a instância é menor que o valor passado no parâmetro, caso sejam iguais ele retornará falso.|
|```isLessThanOrEqualTo(MonetaryAmount amt)```|Retorna verdadeiro se a instância é menor ou igual que o valor passado no parâmetro, caso sejam iguais ele retornará verdadeiro.|
|```boolean isEqualTo(MonetaryAmount amount)```|Retorna verdadeiro caso a instância é igual ao valor monetário passado no parâmetro.|
|```boolean isNegative()```|Retorna verdadeiro se negativo|
|```boolean isNegativeOrZero()```|Retorna verdadeiro se é negativo ou zero|
|```isPositive()```|Retorna verdadeiro se é positivo|
|```boolean isPositiveOrZero()```|Retorna verdadeiro se positivo ou igual a zero|
|```isZero()```|Verifica se o valor do dinheiro é zero.|
|```MonetaryAmount add(MonetaryAmount amount)```|Realiza a soma e retorna o resultado|
|```MonetaryAmount subtract(MonetaryAmount amount)```|Realiza a subtração e retorna o reusltado|
|```MonetaryAmount multiply(Number multiplicand)```|Realiza a multiplicação e retorna o resultado|
|```MonetaryAmount divide(Number divisor)```|Realiza a divisão|
|```MonetaryAmount remainder(Number divisor)```|Realiza a divisão e retorna o resto|
|```MonetaryAmount negate()```|Realiza a negação do montante monetário, ou seja, -this.
|```getCurrency()```|Retorna o dinheiro da moeda.|

Dentro do moneta existem três implementações para a interface ```MonetaryAmount```:


1. 
Money: a implementação padrão, ela representa o valor numérico com o BigDecimal.
1. 
RoundedMoney: assim como a implementação Money, representa o valor numérico com o BigDecimal, a diferença entre eles é que com o RoundedMoney é possível receber um MonetaryOperator para ser chamada a cada operação, por exemplo, a cada operação aritmética realizar uma operação de arredondamento.

1. 
FastMoney: a implementação que representa o valor número com o primitivo long, das implementações apresentadas ela é a mais rápida, cerca de quinze vezes mais rápidas que as outras duas, além de ser mais leve na criação. Porém ela possui uma maior limitação em relação a precisão, caso seja necessário trabalhar com essa precisão, as operações não podem ultrapassar de cinco casas decimais.
