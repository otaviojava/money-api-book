#### Predicate com o valor numérico


Além de operações com moeda é possível realizar condições com o valor numérico do `MonetaryAmount`, os métodos têm o mesmo comportamento da interface do `MonetaryAmount`:


* `MonetaryFunctions.isLessThanOrEqualTo(monetaryAmount)`
* `MonetaryFunctions.isLessThan(monetaryAmount)`
* `MonetaryFunctions.isGreaterThan(monetaryAmount)`
* `MonetaryFunctions.isGreaterThanOrEqualTo(monetaryAmount)`
* `isBetween(MonetaryAmount min, MonetaryAmount max)`


```java
public class PredicateMonetaryAmountNumberValue {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> greaterThanZero = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isGreaterThan(Money.zero(dollar)))
                .collect(Collectors.toList());//[USD 10, USD 10, USD 10, USD 9, USD 8]

        boolean hasAnyGreaterThanZero = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isGreaterThan(Money.zero(dollar)));//true

        boolean allBetweenAndTen = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isBetween(Money.zero(dollar),
                        Money.of(BigDecimal.TEN, dollar)));//true

    System.out.println(greaterThanZero);
    }
}
```