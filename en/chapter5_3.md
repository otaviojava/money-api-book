### Ignoring the exchange rate

If what is need it, is just change the currency, ignoring the rate, there is the method exchange within `MonetaryOperators`.

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