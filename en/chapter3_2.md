### MonetaryQuery


The `MonetaryQuery` similar to `MonetaryOperator` is a funcional interface that receives a `MonetaryOperator`, the difference is on return, the `MonetaryQuery` can return anything, using generics. With the `MonetaryQuery` is possible retrieve some information such the currency code, just the number on `long` or `BigDecimal` format.

```java
public class MonetaryQueryExamples {

    public static void main(String[] args) {
        MonetaryQuery<Long> longQuery = m -> m.getNumber().longValue();
        MonetaryQuery<String> currencyCodeQuery = m -> m.getCurrency().getCurrencyCode();
        MonetaryQuery<Integer> fractionDigits = m -> m.getCurrency().getDefaultFractionDigits();
        

    }
}
```

To execute the `MonetaryQuery` there are two ways, either call the **queryFrom** method on `MonetaryQuery` interface or call the **query** method on `MonetaryAmount`.


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


The Moneta provides some implementations of MonetaryQuery, the MonetaryQueries. It is an utilitarian class that has some queries which make the Java developer life easier, such:

* `MonetaryQuery<Long> extractMajorPart()` retrieves the major part of a money , for example, EUR 2.35 returns 2.
* `MonetaryQuery<Long> convertMinorPart()` Recupera o valor monetário, o convertendo para a menor parte, centavos de um dinheiro, por exemplo, `USD 2.35` será retornado o `235`.


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