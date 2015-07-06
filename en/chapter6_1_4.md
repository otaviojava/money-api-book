#### Multiple Sorters 

The `Comparator.thenComparing` method could be used to chain multiple sorting comparators. Essentially, the secondary comparator will be applied when two values are equal according to the first comparator. This method was added in Java 8. Here's an example.

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