### A classe MonetaryAmountFormatSymbols

Existe também a interface ```MonetaryAmountDecimalFormatBuilder```, que de forma semelhante a classe ```DecimalFormat``` com a classe ```Number```, tem o objetivo de realizar formatações do dinheiro a partir de configurações de símbolos, moedas, quantidade mínima e máxima de dígitos antes e depois da vírgula, etc.


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