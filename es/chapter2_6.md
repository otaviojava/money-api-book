### NumberValue: Una representación de la parte numérica de money


En algunos momentos es importante recuperar y tratar las informaciones de dinero de forma separada, para eso, una interface dispone de un método para recuperar tanto la moneda como el valor monetário. Para la moneda este obviamente devuelve la interface ```CurrencyUnit``` y para representar el valor numérico es devuelto la clase ```NumberValue```. Esta clase es hija de ```Number``` de Java, asi es posible recuperar para los tipos primitivos básicos de Java (```int```, ```long```, ```float```, ```double```, etc.).


```java
public class RetrieveInformationMethods {

    public  static  void main(String[] args) {

        MonetaryAmount money = Money.of(10, Monetary.getCurrency("BRL"));
        CurrencyUnit currency = money.getCurrency();
        Number number = money.getNumber();

    }
}
```

Además de eso, con ```NumberValue``` es posible realizar algunas operaciones más, por ejemplo, es posible definir cuál clase será recuperada a partir de ```NumberValue```, desde que esa clase sea hija de la clase ```Number```. En el caso de la implementación de referencia, para esto serán todas las clases que son ```Number``` y estan dentro de JDK.


```java
public class RetrieveInformationMethods2 {

    public  static  void main(String[] args) {


        MonetaryAmount money = Money.of(BigDecimal.valueOf(10.21), Monetary.getCurrency("BRL"));
        NumberValue number = money.getNumber();
        int precision = number.getPrecision();//4
        int scale = number.getScale();//2
        long amountFractionDenominator = number.getAmountFractionDenominator();//21
        long amountFractionNumerator = number.getAmountFractionNumerator();//10
        Class<?> numberType = number.getNumberType();//java.math.BigDecimal
        BigDecimal value = number.numberValue(BigDecimal.class);
        Integer integerValue = number.numberValue(Integer.class);


    }
}
```
