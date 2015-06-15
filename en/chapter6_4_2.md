#### Partitioning


As well the predicates was explained, it may partition from the money informations with a boolean. The partitioning works on given a condition the values will mapped on true, the `MonetaryAmount` attended this condition, and false, the `MonetaryAmount` doesn't  attended this condition, using the Predicates. Basically the result will be a map whose the keys are `true` and `false` from a `Predicate` condition.


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