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


* **reciprocal()** returns the money as reciprocal, multiply this value by inverse (1/n).
* **permil(Number number)** returns the permil value of the money, for example, permil(10) of `EUR 2.35` returns `EUR 0.0235`.
* **percent(Number number)** returns the percentage of a money, for example, the `percent(10)` of `EUR 200.00` returns `EUR 20.00`.
* **minorPart()** returns the minor part of the money, the value on right of the comma, for example, the minor part of `EUR 2.35` is `EUR 0.35`.
* **majorPart()** returns the integer part of the money, for example, the major part of `EUR 2.35` is `EUR 2`.
* **rounding()** it does the rounding process of the money, to know how much decimal digits the rounding will have will used the **getDefacultFractionDigits** from `CurrencyUnit`.
* **exchange(CurrencyUnit currency)** Given a money this operator just exchange the currency, in other words, just change the currency, but it isn't an exchange rate, for example, the `exchange('BRL')` of `EUR 2.35` returns `BRL 2.35`.

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