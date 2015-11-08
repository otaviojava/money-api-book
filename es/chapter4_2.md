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

Cuando sea necesario configurar las informaciones como cantidad mínima, moneda, etc. Existen dos clases: 

* La primera es la ```MonetaryAmountSymbols``` con esta es posible definir los simbolos que serán utilizados, por ejemplo, símbolo de la moneda, separador, etc. 
* La clase ```MonetaryAmountNumericInformation``` cuidará de las informaciones con relación al formateo del valor numérico, por ejemplo, el número mínimo y máximo de dígitos antes y después de la coma. 

```java
public class MonetaryAmountFormatSymbolsExample2 {

    public static void main(String[] args) {
        MonetaryAmountFormatSymbols defaultFormat = MonetaryAmountFormatSymbols.getDefault();
        MonetaryAmountSymbols amountSymbols = defaultFormat.getAmountSymbols();
        MonetaryAmountNumericInformation numericInformation = defafult.getNumericInformation();
        
    }
}
```



También es posible definir cual implementación será utilizada en la serialización del objeto. Para eso existe la clase funcional ```MonetaryAmountProducer```, con esta es posible definir su propia implementación a partir del número y de la moneda. Moneta por estándar ya viene con tres implementaciones:


* ```FastMoneyProducer``` productor de ```MonetaryAmount``` con la implementación ```FastMoney```.
* ```MoneyProducer``` productor de ```MonetaryAmount``` con la implementación ```Money```.
* ```RoundedMoneyProducer``` productor de ```MonetaryAmount``` con la implementación ```RoundedMoney```, en esta es posible pasar un ```MonetaryOperator``` que será utilizado en la construcción de esa implementación asi no sea informado un operador de redondeo será utilizado ```MonetaryOperators.rounding()```. Recordar que ```MonetaryOperator``` dentro de la implementación ```RoundedMoney``` será utilizado como agente de redondedo, osea, para cada operación ese operador será llamado.



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

También es posible pasar un ```String``` como patrón para el formateo, ese ```String``` sigue el mismo standard de la clase ```DecimalFormat```.

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
