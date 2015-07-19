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