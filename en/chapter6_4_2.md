#### Partitioning

A stream of monetary values could partitioned based on a predicate. The result will be a `Map<Boolean,List<MonetaryAmount>>`where the key is a boolean and the results are based on the behaviour of the predicate as shown in the example below. 

```java
public class AggregatePredicateMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Map<Boolean, List<MonetaryAmount>> mapDollar = Stream.of(money, money2, money3, money4, money5)
                .collect(Collectors.partitioningBy(
                        MonetaryFunctions.isCurrency(dollar)));//{false=[BRL 10, BRL 9], true=[USD 10, USD 10, USD 8]}

    }
}
```