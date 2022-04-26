## Querying/Operation on MonetaryAmount

`MonetaryOperator` and `MonetaryQuery` are functional interfaces. They provide abstraction for querying and operating on monetary values like conversions and retrieving the fractional parts (cents).    

```java

@FunctionalInterface
public interface MonetaryOperator{
  MonetaryAmount apply(MonetaryAmount amount);
}


@FunctionalInterface
public interface MonetaryQuery<R>{
  R queryFrom(MonetaryAmount amount);
}

```

### MonetaryOperator

`MonetaryOperator` is a functional interface that accepts a `MonetaryAmount` and produces another `MonetaryAmount`.
We mentioned this interface when we discussed `RoundedMoney`implementation. With this interface it is possible to do rounding operations, return parts of the money amount, doubles its value, and more.

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
There are two ways to apply the operator. We could either call `MonetaryOperator.apply` or `MonetaryAmount.with` as 
shown in the example below.

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

 **Moneta** provides implementations of `MonetaryOperator`. `MonetaryOperators` provides factory methods for getting instances of  `MonetaryOperator`. These methods are: 

* **reciprocal()** returns the money as reciprocal. Multiply this value by inverse (1/n).
* **permil(Number number)** returns the permil value. For example, permil(10) of `EUR 2.35` returns `EUR 0.0235`.
* **percent(Number number)** returns the percentage. For example, the `percent(10)` of `EUR 200.00` returns `EUR 20.00`.
* **minorPart()** returns the minor part, the value of the fractions. For example, the minor part of `EUR 2.35` is 
`EUR 0.35`.
* **majorPart()** returns the integral part. For example, the major part of `EUR 2.35` is `EUR 2`.
* **rounding()** applies rounding. To see how many decimal digits the rounding will use, see the **getDefaultFractionDigits** from `CurrencyUnit`.
* **exchange(CurrencyUnit currency)** swaps the currency. This operator just change the currency, but it isn't an exchange rate, for example, the `exchange('BRL')` of `EUR 2.35` returns `BRL 2.35`. This method is in the class `ConversionOperators`.

```java

public class MonetaryOperatorsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        CurrencyUnit dollar = Monetary.getCurrency(Locale.US);
        
        MonetaryAmount money = Money.of(120.231, currency);
        
        MonetaryAmount majorPartResult = money.with(MonetaryOperators.majorPart());//BRL 120
        MonetaryAmount minorPartResult = money.with(MonetaryOperators.minorPart());//BRL 0.231
        MonetaryAmount percentResult = money.with(MonetaryOperators.percent(20));//BRL 24.0462
        MonetaryAmount permilResult = money.with(MonetaryOperators.permil(100));//BRL 12.0231
        MonetaryAmount roundingResult = money.with(MonetaryOperators.rounding());//BRL 120.23
        MonetaryAmount resultExchange = money.with(ConversionOperators.exchange(dollar));//USD 120.231
    }
}
```

### MonetaryQuery


Similar to `MonetaryOperator`, `MonetaryQuery` is a functional interface that accepts a  `MonetaryOperator` and returns a generic type-parameter. With `MonetaryQuery` you can perform some queries on the monetary value such as getting the currency code or the numeric value as shown in the example below.

```java
public class MonetaryQueryExamples {

    public static void main(String[] args) {
        MonetaryQuery<Long> longQuery = m -> m.getNumber().longValue();
        MonetaryQuery<String> currencyCodeQuery = m -> m.getCurrency().getCurrencyCode();
        MonetaryQuery<Integer> fractionDigits = m -> m.getCurrency().getDefaultFractionDigits();
        

    }
}
```
There are two ways for applying `MonetaryQuery`, you could either call `MonetaryQuery.queryFrom` or `MonetaryAmount.query` as shown below 

```java
public class HelloMonetaryQuery {

    public  static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, currency);
        MonetaryQuery<String> currencyCodeQuery = m -> m.getCurrency().getCurrencyCode();
        String result = currencyCodeQuery.queryFrom(money);//BRL
        String result2 = money.query(currencyCodeQuery);//BRL
    }
}
```

Moneta provides implementations of `MonetaryQuery` and `MonetaryQueries` provide factory methods for creating instances of type `MonetaryQuery`:

* `MonetaryQuery<Long> extractMajorPart()` returns the major part of a money , for example, EUR 2.35 returns 2.
* `MonetaryQuery<Long> convertMinorPart()` converts the money amount into its full value in cents, for example  `USD 
2.35` becomes `235`.


```java
public class MonetaryQueriesExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency(Locale.US);

        MonetaryAmount money = Money.of(120.23, currency);
        Long moneyInCents = money.query(MonetaryQueries.convertMinorPart());//12023
        Long majorPart = money.query(MonetaryQueries.extractMajorPart());//120
        Long minorPart = money.query(MonetaryQueries.extractMinorPart());//23
    }
}
```

### MonetaryQuery vs MonetaryOperator 

Operations done using `MonetaryQuery` and `MonetaryOperator` might be equivalent. The reason why we have them as separate interfaces is to keep their fuctions separate. `MonetaryQuery` is responsible for retrieving information about the `MonetaryAmount` whereas `MonetaryOperator` is responsible for applying operations on the `MonetaryAmount`. 

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {
        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
