## Money Formatting

`MonetaryAmountFormat` is the interface responsible for formatting the textual representation  of `MonetaryAmount` values. So given a `MonetaryAmount`, `MonetaryAmountFormat.format` is responsible for working out the conversion to a `String` value.

```java
public interface MonetaryAmountFormat extends MonetaryQuery<String>{

   AmountFormatContext getContext();

   default String format(MonetaryAmount amount){...}

   void print(Appendable appendable, MonetaryAmount amount) throws IOException;

   MonetaryAmount parse(CharSequence text) throws MonetaryParseException;

}
```

A simple example is the **toString** and **parse** methods in **Moneta**'s implementation of `MonetaryAmount`.


```java
public class ToStringExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency(Locale.US);
        MonetaryAmount money = Money.of(10, currency);
        MonetaryAmount money2 = FastMoney.of(20, currency);
        MonetaryAmount money3 = RoundedMoney.of(30, currency, MonetaryOperators.rounding());
        String text = money.toString();//USD 10
        String text2 = money2.toString();//USD 20
        String text3 = money3.toString();//USD 30
        MonetaryAmount result = Money.parse(text);
        MonetaryAmount result2 = FastMoney.parse(text2);
        MonetaryAmount result3 = RoundedMoney.parse(text3);

    }
}
```

The reference implementation specifies two ways for creating a formatter. The first option is to create it through a query builder or `Locale`, and the second option is `MonetaryAmountFormatSymbols` which is similar to `java.text.DecimalFormat`. The `MonetaryAmount.parse` method provides an overload that accepts `MonetaryAmountFormat` as a parameter as shown below. 


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

### The class MonetaryAmountFormat

The `MonetaryFormats` class allows the creation of `MonetaryAmountFormats` objects by using either a query builder or by using `Locale`s with the `CurrencyStyle` `enum`.


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
As shown the previous and following examples the `AmountFormatQueryBuilder` allows the creation of custom formats.

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
