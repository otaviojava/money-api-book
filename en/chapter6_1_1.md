#### Sorting by currency

Using sorting using the currency, the **Moneta** will look to the currency code. So given a list of money with the currencies `USD`, `EUR`, `BRL` will return `BRL`, `EUR` and `USD` on ascending way and `USD`, `EUR`, `BRL` on descending way.

```java
public class SortMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnit()).collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnitDesc()).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```

