### The class MonetaryAmountFormatSymbols

The `MonetaryAmountFormatSymbols`s function is similar to `DecimalFormat` with regards to the Number class. The goal is to format the moneys object using a specific configuration. The configuration can take into account currency, minimum and maximum digits, quantities before and after the comma and other properties.

```java
public class MonetaryAmountFormatSymbolsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(12, currency);
        MonetaryAmountFormat defaultFormat = MonetaryAmountFormatSymbols.getDefault();
        String format = defaultFormat.format(money);//R$ 12,00
        MonetaryAmount moneyParsed = Money.parse(format, defaultFormat);//or using defaultFormat.parse(format);

    }
}
```


There are two classes that allow the configuration of the currency or the minimum number of digits to be displayed:

* The first is the `MonetaryAmountSymbols` class. This is used to define the configured symbols; for example currency 
symbols or separator symbols.
* The second is the `MonetaryAmountNumericInformation` class . This contains information about numeric values. An example of this is the minimum and maximum number of digits that can appear before and after the comma.

```java
public class MonetaryAmountFormatSymbolsExample2 {

    public static void main(String[] args) {
        MonetaryAmountFormatSymbols defaultFormat = MonetaryAmountFormatSymbols.getdefaultFormat();
        MonetaryAmountSymbols amountSymbols = defaultFormat.getAmountSymbols();
        MonetaryAmountNumericInformation numericInformation = defaultFormat.getNumericInformation();

    }
}
```

There is the possibility to define which implementation will be used to serialization. The functional interface `MonetaryAmountProducer` allows the creation of `MonetaryAmount`s using a `Number` and a `Currency`. **Moneta** provides three producers, one for each implementation:


* The `FastMoneyProducer` producer, for `MonetaryAmount`s using the `FastMoney` implementation.
* The `MoneyProducer` producer, for `MonetaryAmount`s using the `Money` implementation.
* The `RoundedMoneyProducer` producer, for `MonetaryAmount`s using the `RoundedMoney` implementation. This class has two constructors: the first one has the `MonetaryOperator` as a parameter which will be used in the creation of all `RoundedMoney` objects. The second is the default constructor; it will use `MonetaryOperators.rounding()` as the `MonetaryOperator` for object creation.



```java
public class MonetaryAmountFormatSymbolsExample3 {

    public static void main(String[] args) {
        MonetaryAmountSymbols symbols = new MonetaryAmountSymbols(Locale.US);// new MonetaryAmountSymbols();
        symbols.setCurrencySymbol("Mon");
        MonetaryAmountFormat formatter = MonetaryAmountFormatSymbols.of(symbols, new MoneyProducer());
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        String text = formatter.format(Money.of(10, currency));//Mon 10.00

    }
}
```

Similarly to the `DecimalFormat` class, `MonetaryAmountFormatSymbols` can use a `String` as a formatting pattern. The regex follows the same standardization as in [DecimalFormat](http://docs.oracle.com/javase/7/docs/api/java/text/DecimalFormat.html).

```java
public class MonetaryAmountFormatSymbolsExample4 {

    public static void main(String[] args) {
        MonetaryAmountSymbols symbols = new MonetaryAmountSymbols(Locale.US);// new MonetaryAmountSymbols();
        symbols.setCurrencySymbol("Mon");
        MonetaryAmountFormat formatter = MonetaryAmountFormatSymbols.of("¤ ###,###.00", symbols, new MoneyProducer());
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        String text = formatter.format(Money.of(10_000_00, currency));//Mon 1,000,000.00
    }
}
```
