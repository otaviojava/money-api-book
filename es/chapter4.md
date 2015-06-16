## Formateando Dinero



La interacción con el usuario en la mayoria de los software es una parte fundamental, y asi es necesario pensar en la forma de presentar la información para el usuario además de la interacción con software. Dinero es una parte importante de esos softwares, asi es importante exhibir el gasto total por un servicio, importe total de productos que el usuario desea comprar. Sin hablar en la forma de interacción, por ejemplo, informar el dinero que será transferido por otra cuenta via banca por internet. Para trabajar con el formateo de un ```MonetaryAmount``` existe la interface ```MonetaryAmountFormat``` que básicamente muestra ```MonetaryAmount``` como ```String``` y recupera un ```MonetaryAmount``` a partir de un ```String```. 


```java
public interface MonetaryAmountFormat extends MonetaryQuery<String>{

   AmountFormatContext getContext();

   default String format(MonetaryAmount amount){...}

   void print(Appendable appendable, MonetaryAmount amount) throws IOException;

   MonetaryAmount parse(CharSequence text) throws MonetaryParseException;

}
```

Um ejemplo simple es **toString** y el parseo dentro de las implementaciones de ```MonetaryAmount``` dentro de Moneta.


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
