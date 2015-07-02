### Ignoring the exchange rate

In order to just the change currency, ignoring the rate, the method exchange from the `MonetaryOperators` class can be used.

```java
public class ExchangeRateProviderExample5 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);

        MonetaryOperator operator = MonetaryOperators.exchange(dollar);
        MonetaryAmount result = money2.with(operator).add(money);//USD 20.00000 ignoring currency
    }
}
```
