### MonetaryOperator: 


The `MonetaryOperator` is an functional interface that receive a `MonetaryAmount` and then returns a `MonetaryAmount`. This interface was told when discussed about the `RoundedMoney` implementation. With this interface is possible does rounding operations, return some part of the money, the double of the money, etc.

```java
public class MonetaryOperatorExamples {

    public  static void main(String[] args) {
        MonetaryOperator doubleOperator = m -> m.multiply(2);
        MonetaryOperator halfOperator = m -> m.divide(2);
        MonetaryOperator operator = m -> {
            if(m.isPositive()){
                return m.multiply(2);
            }
            return m.divide(2);
        };
    }
}
```

To execute the `MonetaryOperator` there are two ways, either call the **apply** method on `MonetaryOperator` interface or call the **with** method on `MonetaryAmount`.


```java
public class HelloMonetaryOperator {

    public  static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, currency);
        MonetaryOperator doubleOperator = m -> m.multiply(2);
        MonetaryAmount result = doubleOperator.apply(money);//BRL 20.00000
        MonetaryAmount result2 = result.with(doubleOperator);//BRL 40.00000
    }
}
```


The **Moneta** provides some implementations of `MonetaryOperator` on `MonetaryOperators` class. It is a utilitarian class that has some functionalities really trivial to all Java developers who will work with money such:


* **reciprocal()** Retorna o dinheiro como reciprocal, multiplicando pelo valor inverso (1/n).
* **permil(Number number)** retorna o valor permil do dinheiro, por exemplo, **permil(10)** de `EUR 2.35` retornará EUR `0.0235`.
* **percent(Number number)** retorna o percentual de um dinheiro, por exemplo, **percent(10)** de `EUR 200.00` retornará `EUR 20.00`.
* **minorPart()** retorna o valor que se encontra na direita da vírgula, por exemplo, a menor parte de `EUR 2.35` é ```EUR 0.35```.
* **majorPart()** retorna o valor inteiro do dinheiro, por exemplo, a menor parte de `EUR 2.35` é `EUR 2`.
* **rounding()** realiza o processo de arredondamento do dinheiro, para saber o número de casas após a vírgula é recuperado a informação do método getDefaultFractionDigits() da interface CurrencyUnit.
* **exchange(CurrencyUnit currency)** dado um dinheiro esse operador realizará a troca da moeda, ou seja, ele apenas vai trocar mudar a moeda não levando em consideração a sua cotação, por exemplo, `EUR 2.35` **exchange('BRL')** retornará `BRL 2.35`.

```java

public class MonetaryOperatorsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        CurrencyUnit dollar = Monetary.getCurrency(Locale.US);
        
        MonetaryAmount money = Money.of(120.231, currency);
        
        MonetaryAmount majorParteResult = money.with(MonetaryOperators.majorPart());//BRL 120
        MonetaryAmount minorPartResult = money.with(MonetaryOperators.minorPart());//BRL 0.231
        MonetaryAmount percentResult = money.with(MonetaryOperators.percent(20));//BRL 24.0462
        MonetaryAmount permilResult = money.with(MonetaryOperators.permil(100));//BRL 12.0231
        MonetaryAmount roundingResult = money.with(MonetaryOperators.rounding());//BRL 120.23
        MonetaryAmount resultExchange = money.with(MonetaryOperators.exchange(dollar));//USD 120.231
    }
}
```