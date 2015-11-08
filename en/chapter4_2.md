### The class MonetaryAmountFormatSymbols

The `MonetaryAmountDecimalFormatBuilder`s function is similar to `DecimalFormat` with regards to the Number class. The goal is to format the moneys object using a specific configuration. The configuration can take into account currency, minimum and maximum digits, quantities before and after the comma and other properties.

```java
public class MonetaryAmountDecimalFormatBuilderExample {

    public static void main(String[] args) {
    	MonetaryAmountFormat defaultFormat = MonetaryAmountDecimalFormatBuilder.newInstance().build();
    	MonetaryAmountFormat patternFormat = MonetaryAmountDecimalFormatBuilder.of("¤ ###,###.00").build();
    	MonetaryAmountFormat localeFormat = MonetaryAmountDecimalFormatBuilder.of(Locale.US).build();
    	
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(12, currency);
        String format = defaultFormat.format(money);//$12.00
        MonetaryAmount moneyParsed = Money.parse(format, defaultFormat);//or using defafult.parse(format);

    }
}

```


There is the possibility to define which implementation will be used to serialization. The functional interface `MonetaryAmountProducer` allows the creation of `MonetaryAmount`s using a `Number` and a `Currency`. **Moneta** provides three producers, one for each implementation:


* The `FastMoneyProducer` producer, for `MonetaryAmount`s using the `FastMoney` implementation.
* The `MoneyProducer` producer, for `MonetaryAmount`s using the `Money` implementation.
* The `RoundedMoneyProducer` producer, for `MonetaryAmount`s using the `RoundedMoney` implementation. This class has two constructors: the first one has the `MonetaryOperator` as a parameter which will be used in the creation of all `RoundedMoney` objects. The second is the default constructor; it will use `MonetaryOperators.rounding()` as the `MonetaryOperator` for object creation.



```java
public class MonetaryAmountDecimalFormatBuilderExample2 {

    public static void main(String[] args) {
    	CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(12, currency);
        
    	MonetaryAmountFormat formater = MonetaryAmountDecimalFormatBuilder.of(new Locale("pt", "BR")).
    			withCurrencyUnit(currency).withProducer(new MoneyProducer()).build();
    	
        
        String format = formater.format(money);//R$ 12,00
        MonetaryAmount moneyParsed = Money.parse(format, formater);//or using defafult.parse(format);

    }
}
```

Similarly to the `DecimalFormat` class, `MonetaryAmountFormatSymbols` can use a `String` as a formatting pattern. The regex follows the same standardization as in [DecimalFormat](http://docs.oracle.com/javase/7/docs/api/java/text/DecimalFormat.html).


```java
public class MonetaryAmountDecimalFormatBuilderExample3 {

    public static void main(String[] args) {
    	MonetaryAmountFormat patternFormat = MonetaryAmountDecimalFormatBuilder.of("¤ ###,###.00").build();
    	
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(12, currency);
        String format = patternFormat.format(money);//$ 12.00
        MonetaryAmount moneyParsed = Money.parse(format, patternFormat);//or using defafult.parse(format);

    }
}

```
