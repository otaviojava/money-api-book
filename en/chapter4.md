## Formating money


The user's iteration in the software is a fundamental part, so is necessary think the way to show the information to the user besides interacting with the software. The money is a significant part of a software, so is vital show the total cost of a service or the total of products that will be purchased or the money that will be borrowed using an Internet bank. To work with formatting of a `MonetaryAmount` exist a `MonetaryAmountFormat`'s interface that basically given a `MonetaryAmount` convert to `String` and given the `String` returns a `MonetaryAmount`.



```java
public interface MonetaryAmountFormat extends MonetaryQuery<String>{

   AmountFormatContext getContext();

   default String format(MonetaryAmount amount){...}

   void print(Appendable appendable, MonetaryAmount amount) throws IOException;

   MonetaryAmount parse(CharSequence text) throws MonetaryParseException;

}
```

A simple example is the **toString** method and the parse inside of all implementation of `MonetaryAmount` on **Moneta**.


```java
public class ToStrimgExample {

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

A implementação de referência tem duas formas de criar formatador para dinheiro. A primeira opção é com a classe `MonetaryFormats` com ela é possível criar formatador a partir de uma query builder ou utilizando apenas pelo `Locale`. A segunda opção é utilziando a classe `MonetaryAmountFormatSymbols` que tem o seu funcionamento semelhante a classe `DecimalFormat`. Além do parse sem parâmetros nas implementações do MonetaryAmount, também existe o método parse que aceita o MonetaryAmountFormat.


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