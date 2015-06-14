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

Com o `AmountFormatQueryBuilder` é possível também, criar formatos customizados.

```java
public class MonetaryFormatsExampleQueryCustom {

    public static void main(String[] args) {
    	MonetaryAmount amount = Money.of(12345.67, "USD");
    	MonetaryAmountFormat customFormat = MonetaryFormats.getAmountFormat(
    			AmountFormatQueryBuilder.of(Locale.US)
    			.set(CurrencyStyle.NAME)
    			.set("pattern", "00,00,00,00.00 ¤")
    			.build()); 

    			String formatted = customFormat.format(amount); //00,01,23,45.67 US Dollar
    }
}
```
