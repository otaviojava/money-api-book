## A classe MonetaryAmountFormat

A implementação de referência tem duas formas de criar formatador para dinheiro. A primeira opção é com a classe ```MonetaryFormats``` com ela é possível criar formatador a partir de uma query builder ou utilizando apenas pelo ```Locale```. Além do parse sem parâmetros nas implementações do ```MonetaryAmount```, também existe o método parse que aceita o ```MonetaryAmountFormat```.

```java
public class MonetaryFormatsExample {


    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("EUR");
        MonetaryAmount money = Money.of(12, currency);

        MonetaryAmountFormat format =
                MonetaryFormats.getAmountFormat(Locale.US);

        String resultText = format.format(money);//EUR 12
        MonetaryAmount monetaryAmount = format.parse(resultText);
        MonetaryAmount result2 = Money.parse(resultText, format);

    }
}
```



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
