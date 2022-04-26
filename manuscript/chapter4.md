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

Um exemplo simples é o **toString** e o parse dentro das implementações do ```MonetaryAmount``` dentro do moneta.


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

### A classe MonetaryAmountFormat

Com o `MonetaryFormats` é possível criar formatador a partir de uma query builder ou utilizando apenas pelo `Locale`. Com o `enum CurrencyStyle` é possível informar quais informações entrarão no formatador.


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

Com o `AmountFormatQueryBuilder` é possível também, criar formatos customizados.

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


### A classe MonetaryAmountFormatSymbols

Existe também a classe ```MonetaryAmountDecimalFormatBuilder```, que de forma semelhante a classe ```DecimalFormat``` com a classe ```Number```, tem o objetivo de realizar formatações do dinheiro a partir de configurações de símbolos, moedas, quantidade mínima e máxima de dígitos antes e depois da vírgula, etc.


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


Também é possível definir qual implementação que será utilizada na serialização do objeto. Para isso existe a classe funcional ```MonetaryAmountProducer```, com ela é possível definir sua própria implementação a partir do number e da moeda. O **Moneta** por padrão já vem com três implementações:


* ```FastMoneyProducer``` produtor de ```MonetaryAmount``` com a implementação ```FastMoney```.
* ```MoneyProducer``` produtor de ```MonetaryAmount``` com a implementação ```Money```.
* ```RoundedMoneyProducer``` produtor de ```MonetaryAmount``` com a implementação ```RoundedMoney```, nela é possível passar um ```MonetaryOperator``` que será utilizado na construção dessa implementação caso não seja informado um operador de arredondamento será utilizado o ```MonetaryOperators.rounding()```. Lembrando que ```MonetaryOperator``` dentro da implementação ```RoundedMoney``` será utilizado como agente arredondador, ou seja, para cada operação esse operador será chamado.



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

Também é possível passar uma ```String``` como pattern para a formatação, essa ```String``` segue o mesmo padrão da classe [DecimalFormat](http://docs.oracle.com/javase/7/docs/api/java/text/DecimalFormat.html).


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