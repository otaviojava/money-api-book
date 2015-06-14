### Exchange rate from a specific data

For some reasons in the application is important to know not just the exchange rate most recent, but an exchange rate from a distinct date, from example, when rent a room in the hotel, normally will be used the rate when the room was reserved or in the credit card that use the rate when the bill was closed. **Moneta** gives support to do it, just use the `ConversationQuery` class, it is possible find out with defined date or a range of dates. The representation that is accepted for date is the `LocalDate` class.


```java
public class ExchangeRateProviderExample2 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        LocalDate localDate = Year.of(2009).atMonth(Month.JANUARY).atDay(9);

        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF_HIST);
        ConversionQuery query = ConversionQueryBuilder.of().setTermCurrency(dollar).set(localDate).build();

        CurrencyConversion currencyConversion = provider.getCurrencyConversion(query);

        MonetaryAmount result = currencyConversion.apply(money2);
        MonetaryAmount monetaryAmount = money.add(result);
        System.out.println(monetaryAmount);
    }
}
```

Caso a data especificada não seja encontrada será retornada uma exceção, por exemplo, não será possível recuperar a cotação do dia 9 de janeiro de 2011, uma vez que essa data foi em um domingo e a grande maioria dos provedores de cotação não trabalham nesse dia.

```java
public class ExchangeRateProviderExample3 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        LocalDate localDate = Year.of(2011).atMonth(Month.JANUARY).atDay(9);

        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF_HIST);
        ConversionQuery query = ConversionQueryBuilder.of().setTermCurrency(dollar).set(localDate).build();

        CurrencyConversion currencyConversion = provider.getCurrencyConversion(query);

        MonetaryAmount result = currencyConversion.apply(money2);//javax.money.MonetaryException: There is not exchange on day 2011-01-09 to rate to  rate on IFMRateProvider.


    }
}
```


Uma possível solução para esse problema é passar um range de datas, assim a implementação vai procurar algumas das datas, caso não encontre nenhuma delas lançará uma exceção, vale salientar que a implementação buscará a partir da ordem que foi definida.

```java
public class ExchangeRateProviderExample4 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        LocalDate localDate = Year.of(2011).atMonth(Month.JANUARY).atDay(9);
        LocalDate[] localDates = Stream.of(localDate, localDate.minusDays(1L), localDate.minusDays(2L),
                localDate.minusDays(3L)).sorted(Comparator.<LocalDate>naturalOrder().reversed()).toArray(LocalDate[]::new);
        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF_HIST);
        ConversionQuery query = ConversionQueryBuilder.of().setTermCurrency(dollar).set(localDates).build();

        CurrencyConversion currencyConversion = provider.getCurrencyConversion(query);

        MonetaryAmount result = currencyConversion.apply(money2);
        MonetaryAmount sum = money.add(result);

    }
}
```