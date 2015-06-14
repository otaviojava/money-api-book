### Using the ExchangeRateProvider

The **money-api** provides a way to do exchange rate, the `ExchangeRateProvider` interface represents this action.

```java
public class ExchangeRateProviderExample {

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



Inside the Moneta there are five implementations of `ExchangeRateProvider`:

* **ECB** implementation that retrieve the rate most recent from Europe Central Bank.
* **IMF** implementation that retrieve the rate most recent from International Monetary Fund.
* **IMF_HIST** implementation that retrieve the rate of a specific date from Europe Central Bank.
* **ECB_HIST90** implementation that retrieve the last ninety days of rate from Europe Central Bank.
* **ECB_HIST** implementation that retrieve all information since 1999 from Europe Central Bank.

