### Exchange rate from a specific data

Applications may need to know not only the most recent exchange rate, but exchange rates from specific dates. For example: when renting a room in a hotel, in order to create the bill the exchange rate from the date when the room was booked would be needed. **Moneta** supports this, by using the `ConversationQuery` class it is possible find out the exchange rate for a defined date or for a range of dates. `LocalDate` class should be used to represent dates.


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

If **Moneta** cannot find the date, it will throw an exception. An example, it will not possible to find a rate for the 9th of January, 2011. This date represents a Sunday and almost always providers will not have an entry for this day.


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

A possible solution to this problem is to use a range of dates. This way, the call has a higher chance of succeeding. The implementation will attempt to find out the rates for all provided dates. It will return the rates in the same order the dates were given. If it doesn't find any rates for the provided dates it will throw an exception.

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
