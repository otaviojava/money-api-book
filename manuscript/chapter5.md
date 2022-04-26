## Exchange Rate

Internationalization is the process of increasing the involvement of enterprises in international markets. This can have an impact on multiple levels in culture, economy or politics. It affects daily life in that it's easy to find out what is happening across the world, to discover cultures just by surfing the Internet or to simply buy products or services from other countries. Software needs to follow this trend, it needs to allow users to easily interact with each other across the globe. It is exactly the same case with money; software has an obligation to support different currencies.


Talking about money and internationalization will lead you to think about the interaction between different currencies. What would happen if we tried to do the following operation?

```java
public class MistakeExample {


    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);
        MonetaryAmount result = money.add(money2);//javax.money.MonetaryException: Currency mismatch: USD/BRL
    }
}
```

Using **Moneta**, you will get an exception with the error informing you that you are trying to sum money with distinct currencies. It is a “correct error” even if the dollar has the exact same value as one Brazilian real. In order for this summing operation to succeed, it first needs to use the exchange rate between the two currencies. The exchange rate is the rate at which one currency will be exchanged for another.


### Using the ExchangeRateProvider

The **money-api** has a way to use exchange rates: the `ExchangeRateProvider` interface.

```java
public class ExchangeRateProviderExample1 {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);
        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.ECB);
        CurrencyConversion currencyConversion = provider.getCurrencyConversion(dollar);
        MonetaryAmount result = currencyConversion.apply(money2);//value on dollar
        MonetaryAmount monetaryAmount = money.add(result);//result on dollar
    }
}
```



Moneta has five implementations of `ExchangeRateProvider`:

* **ECB** - Is an implementation that retrieves the latest rate from the European Central Bank.
* **IMF** - Is an implementation that retrieves the latest rate from the International Monetary Fund.
* **IMF_HIST** - Is an implementation that retrieves the rate of a specific date from the International Monetary Fund.
* **ECB_HIST90** - Is an implementation that retrieves the last ninety days of rates from the European Central Bank.
* **ECB_HIST** - Is an implementation that retrieves all available information since 1999 from the European Central Bank.

### Exchange rate from a specific data

Applications may need to know not only the most recent exchange rate, but exchange rates from specific dates. For example: when renting a room in a hotel, the exchange rate from the date when the room was booked would be needed to create the bill. **Moneta** supports this. By using the `ConversationQuery` class, it is possible find out the exchange rate for a defined date or for a range of dates. `LocalDate` class should be used to represent dates.


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

If **Moneta** cannot find the date, it will throw an exception. For example, it will not be possible to find a rate for the 9th of January, 2011. This date is a Sunday and as such almost all providers will not have an entry for this day.


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

### Ignoring the exchange rate

In order to just change the currency whilst ignoring the rate, the method exchange from the `ConversionOperators` class can be used.

```java
public class ExchangeRateProviderExample5 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);

        MonetaryOperator operator = ConversionOperators.exchange(dollar);
        MonetaryAmount result = money2.with(operator).add(money);//USD 20.00000 ignoring currency
    }
}
```

