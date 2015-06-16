#### Realizando ordenación con valor numérico

La ordenación por el valor numérico ignora la moneda y ordena solo teniendo en consideración el valor monetário, cabe resaltar, que esa ordenación no realiza cotización de valores, en otras palabras, el valor de diez reales tendrá el mismo valor que diez dolares. También es posible retornar de forma ascendente y descendente.

```java
public class SortMonetaryAmountNumber {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortNumber()).collect(Collectors.toList());//[EUR 9, USD 10, BRL 11]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortNumberDesc()).collect(Collectors.toList());//[BRL 11, USD 10, EUR 9]
    }
}
```