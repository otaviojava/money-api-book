#### Agrupando por la moneda


Moneta trae una función para facilitar el agrupamiento de varios `MonetaryAmount` a partir de la moneda.


```java
public class AggregateGroupByCurrencyMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Map<CurrencyUnit, List<MonetaryAmount>> groupByCurrency = Stream.of(money, money2, money3, money4, money5)
                .collect(MonetaryFunctions.groupByCurrencyUnit());//{USD=[USD 10, USD 10, USD 8], BRL=[BRL 10, BRL 9]}


    }
}
```