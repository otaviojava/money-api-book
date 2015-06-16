#### Particionamiento


Como los predicates, explicados del subcapítulo anterior, es posible participar las informaciones de dinero. El particionamiento se basa en dada una condición los valores serán mapeados en true, los MonetaryAmounts que cumplieron la condición, y false, los `MonetaryAmounts` que no cumplieron la condición.


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