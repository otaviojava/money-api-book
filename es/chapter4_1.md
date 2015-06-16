### La clase MonetaryAmountFormat

La implementación de referencia tiene dos formas de crear formato para Dinero. La primera opción es con la clase  ```MonetaryFormats``` con esta es posible crear un formateador a partir de un query builder o utilizando solo por ```Locale```. Además del parseo sin parametros en las implementaciones de ```MonetaryAmount```, tambien existe el metodo parse que acepta ```MonetaryAmountFormat```.


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
