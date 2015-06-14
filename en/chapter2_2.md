### Representing money and currency with money-api



On money-api, will used this name instead of JSR 354, also will be necessary to represent both the numeric value and currency. To represents the currency there is the CurrencyUnit interface all implementation must be immutable and thread-safe.

|Method's name| Description |Example|
| -- | -- | -- |
|```String getCurrencyCode()```|Returns the currency code, to currency that follows the ISO will be returned three words.|BRL to Brazilian Real and USD dollar.
|```int getNumericCode()```|Returns the numeric code of the currency, similar to currency code it has three digits.|986 to Brazilian Real and 840 to dollar.|
|``` int getDefaultFractionDigits()``` |Returns the number of digits normally used by currency.|BRL has two digits and JPY hasn't.|


This book will use the **Moneta** implementation, the reference implementation to this specification. Using the **Moneta** is possible have the currency's instance on two ways. The first way is using the currency code, so will use the ```String``` with three words.


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

The simplest way is using the `Locale` class, this option is very interesting when, for example, a web application knows, from `Locale` on request, the currency of the user that is accessing the application.

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

Once was discussed about the currency the next step will talk about the representation of the monetary amount, for this, there is the `MonetaryAmount`'s interface, an important behavior is all implementation must be immutable and thread-safe.

|Method| Description|
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
