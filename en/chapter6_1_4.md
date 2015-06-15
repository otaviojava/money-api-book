#### Mixing the sorters



Just to remember, because this feature isn't of the money-api but from Java 8, it may mix two or more sorter, to do it just uses the **thenComparing** method. Basically it does a sort operation and when returns zero, it will use the other sorter, so the way it is putted the sorter will change the sort result.


```java
public class SortMixMonetaryAmountNumberCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(10, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, real);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortNumber().thenComparing(MonetaryFunctions.sortCurrencyUnit()))
                .collect(Collectors.toList());//[USD 8, BRL 9, BRL 10, EUR 10, USD 10]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortNumberDesc().thenComparing(MonetaryFunctions.sortCurrencyUnitDesc()))
                .collect(Collectors.toList());//[USD 10, EUR 10, BRL 10, BRL 9, USD 8]
        //using currency first
        List<MonetaryAmount> resultCurrencyAsc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnit().thenComparing(MonetaryFunctions.sortNumber()))
                .collect(Collectors.toList());//[BRL 9, BRL 10, EUR 10, USD 8, USD 10]
        List<MonetaryAmount> resultCurrencyDesc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnitDesc().thenComparing(MonetaryFunctions.sortNumberDesc()))
                .collect(Collectors.toList());//[USD 10, USD 8, EUR 10, BRL 10, BRL 9]

    }
}
```