### MonetaryQuery


```MonetaryQuery``` semejante a ```MonetaryOperator``` es una interface funcional que recibe un ```MonetaryOperator```, la diferencia está en el retorno, este puede devolver cualquier tipo de objeto a partir de *generics*. Con ```MonateryQuery``` es posible recuperar algunas informaciones en el ```MonetaryAmount```, por ejemplo, el código de la moneda, solo el número en el  formato long o en BigDecimal.

```java
public class MonetaryQueryExamples {

    public static void main(String[] args) {
        MonetaryQuery<Long> longQuery = m -> m.getNumber().longValue();
        MonetaryQuery<String> currencyCodeQuery = m -> m.getCurrency().getCurrencyCode();
        MonetaryQuery<Integer> fractionDigits = m -> m.getCurrency().getDefaultFractionDigits();
        

    }
}
```

Para ejecutarlo basta llamar al método apply de la interface o llamar al método “with” dentro de MonetaryAmount.


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


Moneta trae como estándar algunas implementaciones de ```MonetaryQuery```, la clase ```MonetaryQueries```. Esta es una clase utilitaria que trae algunas funcionalidades importantes y algunas veces muy cotidianas dentro de la vida de un desarrollador Java, como:

* **MonetaryQuery<Long> extractMajorPart()** recupera la mayor parte de dinero, por ejemplo, `EUR 2.35` retornará `2`.
* **MonetaryQuery<Long> convertMinorPart()** recupera el valor monetário, o convirtiendo para la menor parte, centavos de dinero, por ejemplo, `USD 2.35` será retornado `235`.


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
