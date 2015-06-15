#### Realizando ordenação levando em consideração a cotação


Também é possível realizar uma ordenação de forma crescente e decrescente levanto em consideração a cotação da moeda. Para isso basta passar uma implementação de `ExchangeRateProvider`. Por exemplo, dado uma lista com dez dólares, onze reais e nove euros, retornará de forma ascendente o valor de onze reais, dez dólares e nove euros levando em consideração que pela cotação o dólar é mais valioso que o real e menos que valioso que o euro.


```java
public class SortMonetaryAmountExchange {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        ExchangeRateProvider provider =
                MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF);

    
        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValuable(provider))
                .collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]

        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValuableDesc(provider)).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```