    ### La clase MonetaryAmountFormatSymbols

Existe también la classe ```MonetaryAmountDecimalFormatBuilder```, que de forma semejante la clase ```DecimalFormat``` con la clase ```Number```, tiene el objetivo de realizar formateadores de dinero a partir del as configuraciones de símbolos, monedas, cantidad mínima y máxima de dígitos antes y despues la coma decimal, etc.


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


También es posible definir cual implementación será utilizada en la serialización del objeto. Para eso existe la clase funcional ```MonetaryAmountProducer```, con esta es posible definir su propia implementación a partir del número y de la moneda. Moneta por estándar ya viene con tres implementaciones:


* ```FastMoneyProducer``` productor de ```MonetaryAmount``` con la implementación ```FastMoney```.
* ```MoneyProducer``` productor de ```MonetaryAmount``` con la implementación ```Money```.
* ```RoundedMoneyProducer``` productor de ```MonetaryAmount``` con la implementación ```RoundedMoney```, en esta es posible pasar un ```MonetaryOperator``` que será utilizado en la construcción de esa implementación asi no sea informado un operador de redondeo será utilizado ```MonetaryOperators.rounding()```. Recordar que ```MonetaryOperator``` dentro de la implementación ```RoundedMoney``` será utilizado como agente de redondedo, osea, para cada operación ese operador será llamado.



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

También es posible pasar un ```String``` como patrón para el formateo, ese ```String``` sigue el mismo standard de la clase ```DecimalFormat```.


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
