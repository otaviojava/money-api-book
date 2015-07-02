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
* **IMF_HIST** - Is an implementation that retrieves the rate of a specific date from the European Central Bank.
* **ECB_HIST90** - Is an implementation that retrieves the last ninety days of rates from the European Central Bank.
* **ECB_HIST** - Is an implementation that retrieves all available information since 1999 from the European Central Bank.
