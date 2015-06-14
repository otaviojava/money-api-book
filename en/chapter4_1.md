### The class MonetaryAmountFormat

Using the `MonetaryFormats` may to create from a query builder or just using from `Locale` with the `CurrencyStyle` `enum` may put informations that will show on formatter. 


```java
public class MonetaryFormatsExampleQuery {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("USD");
        MonetaryAmount money = Money.of(12, currency);

        MonetaryAmountFormat format = MonetaryFormats
                        .getAmountFormat(AmountFormatQueryBuilder.of(Locale.US).set(CurrencyStyle.SYMBOL).build());


        String resultText = format.format(money);//$12.00
    }
}
```
