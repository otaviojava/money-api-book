### Ignorando la cotización

En algunos momentos es necesario realizar un intercambio de moneda sin realizar la cotización, para eso existe el método **exchange** dentro de ```ConversionOperators```.

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