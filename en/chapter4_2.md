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



Também é possível definir qual implementação que será utilizada na serialização do objeto. Para isso existe a classe funcional ```MonetaryAmountProducer```, com ela é possível definir sua própria implementação a partir do number e da moeda. O moneta por padrão já vem com três implementações:


* ```FastMoneyProducer``` produtor de ```MonetaryAmount``` com a implementação ```FastMoney```.
* ```MoneyProducer``` produtor de ```MonetaryAmount``` com a implementação ```Money```.
* ```RoundedMoneyProducer``` produtor de ```MonetaryAmount``` com a implementação ```RoundedMoney```, nela é possível passar um ```MonetaryOperator``` que será utilizado na construção dessa implementação caso não seja informado um operador de arredondamento será utilizado o ```MonetaryOperators.rounding()```. Lembrando que ```MonetaryOperator``` dentro da implementação ```RoundedMoney``` será utilizado como agente arredondador, ou seja, para cada operação esse operador será chamado.



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

Também é possível passar uma ```String``` como pattern para a formatação, essa ```String``` segue o mesmo padrão da classe ```DecimalFormat```.

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