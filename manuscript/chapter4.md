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