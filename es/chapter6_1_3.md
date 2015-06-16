#### Realizando ordenación teniendo en consideración la cotización


Tambien es posible realizar una ordenación de forma creciente y decreciente teniendo en consideración la cotización de la moneda. Para esto basta pasar una implementación de `ExchangeRateProvider`. Por ejemplo, dado una lista com diez dolares, once reales y nueve euros, retornará de forma ascendente el valor de once reales, diez dólares y nueve euros teniendo en consideración que por la cotización el dolar es mas valioso que el real y menos valioso que el euro.


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
                        .sortValiable(provider))
                .collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]

        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValiableDesc(provider)).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```