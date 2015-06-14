## Formatando dinheiro



A interação com o usuário na maioria dos software é uma parte fundamental, assim é necessário pensar na forma de apresentar a informação para o usuário além da interação com o software. O dinheiro é uma parte importante desses softwares, assim é importante exibir o total gasto por um serviço ou o somatório de produtos que o usuário deseja comprar. Sem falar na forma de interação, por exemplo, informar o dinheiro que será transferido por uma outra conta via internet banking. Para trabalhar com a formatação de um ```MonetaryAmount``` existe a interface ```MonetaryAmountFormat``` que basicamente expõe o ```MonetaryAmount``` como ```String``` e recupera um ```MonetaryAmount``` a partir de uma ```String```. 


```java
public interface MonetaryAmountFormat extends MonetaryQuery<String>{

   AmountFormatContext getContext();

   default String format(MonetaryAmount amount){...}

   void print(Appendable appendable, MonetaryAmount amount) throws IOException;

   MonetaryAmount parse(CharSequence text) throws MonetaryParseException;

}
```

Um exemplo simples é o **toString** e o parse dentro das implementações do ```MonetaryAmount``` dentro do **Moneta**.


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