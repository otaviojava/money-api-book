### NumberValue: The representation of numeric part of the money.

Some moments, is important retrieve and take care the money information separately, to does that, the interface has methods to just return both currency and the numeric value. To the currency will return an implementation of `CurrencyUnit` and to represent the numeric value will return the `NumberValue` class. This class extends `Number`, so is possible parse it to almost all primitives types on Java (`int`, `long`, `float`, `double`, etc.).


```java
public class RetrieveInformationMethods {

    public  static  void main(String[] args) {

        MonetaryAmount money = Money.of(10, Monetary.getCurrency("BRL"));
        CurrencyUnit currency = money.getCurrency();
        Number number = money.getNumber();

    }
}
```

With `NumberValue` also is possible do one more operation, for example, is possible define which class will returned from `NumberValue`, once this class be son of `Number`. On reference implementation all classes that extend `Number` on **JDK** can be returned.



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