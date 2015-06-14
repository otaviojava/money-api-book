### A classe MonetaryAmountFormat

Com o `MonetaryFormats` é possível criar formatador a partir de uma query builder ou utilizando apenas pelo `Locale`. Com o `enum CurrencyStyle` é possível informar quais informações entrarão no formatador.


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
