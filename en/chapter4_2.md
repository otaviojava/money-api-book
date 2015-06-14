### The class MonetaryAmountFormatSymbols

The `MonetaryAmountFormatSymbols`  that has the behavior look like `DecimalFormat` with the Number class, the goal is formating the money from some configurations such currency, minimum and maximum digits quantities after and before the comma, etc.

```java
public class MonetaryAmountFormatSymbolsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(12, currency);
        MonetaryAmountFormat defafult = MonetaryAmountFormatSymbols.getDefafult();
        String format = defafult.format(money);//R$ 12,00
        MonetaryAmount moneyParsed = Money.parse(format, defafult);//or using defafult.parse(format);

    }
}
```


Using this class may change the settings about minimum quantity, currency, etc. there are two classes to do that:

* The first is `MonetaryAmountSymbols` with it is may to define the symbols that will be used, for example, currency symbols, separator symbols. 
* The `MonetaryAmountNumericInformation` takes care of the informations about the numeric value like minimum and maximum digits after and before the comma.

```java
public class MonetaryAmountFormatSymbolsExample2 {

    public static void main(String[] args) {
        MonetaryAmountFormatSymbols defafult = MonetaryAmountFormatSymbols.getDefafult();
        MonetaryAmountSymbols amountSymbols = defafult.getAmountSymbols();
        MonetaryAmountNumericInformation numericInformation = defafult.getNumericInformation();
        
    }
}
```



There is the possibility to define which implementation will be used to serialization. To do it, exist the functional class `MonetaryAmountProduce`, it may define own implementation from `Number` and `Currency`. The **Moneta** provides three producer one to each implementation that it already has:


* `FastMoneyProducer` producer of `MonetaryAmount` using the `FastMoney` implementation.
* `MoneyProducer` producer of `MonetaryAmount` using the `Money` implementation.
* `RoundedMoneyProducer` producer of `MonetaryAmount` using the `RoundedMoney` implementation. This class has two constructors: the first one has the `MonetaryOperator` as parameter, this parameter will be used on creation of all `RoundedMoney` created in this class. The second is the default constructor, without parameter, so will use `MonetaryOperators.rounding()` as `MonetaryOperator` that will use on all of `MonetaryAmount` creation using the `RoundedMoney` implementation.



```java
public class MonetaryAmountFormatSymbolsExample3 {

    public static void main(String[] args) {
        MonetaryAmountSymbols symbols = new MonetaryAmountSymbols(Locale.US);// new MonetaryAmountSymbols();
        symbols.setCurrencySymbol("Mon");
        MonetaryAmountFormat formater = MonetaryAmountFormatSymbols.of(symbols, new MoneyProducer());
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        String text = formater.format(Money.of(10, currency));//Mon 10.00

    }
}
```

As on `DecimalFormat` class, it may to use a `String` as formating pattern, the regex follows the same standardization of `DecimalFormat`.

```java
public class MonetaryAmountFormatSymbolsExample3 {

    public static void main(String[] args) {
        MonetaryAmountSymbols symbols = new MonetaryAmountSymbols(Locale.US);// new MonetaryAmountSymbols();
        symbols.setCurrencySymbol("Mon");
        MonetaryAmountFormat formater = MonetaryAmountFormatSymbols.of("¤ ###,###.00", symbols, new MoneyProducer());
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        String text = formater.format(Money.of(10_000_00, currency));//Mon 1,000,000.00
        System.out.println(text);
    }
}
```