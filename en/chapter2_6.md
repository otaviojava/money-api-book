### NumberValue: The Numerical Part

It is possible to retrieve the numeric value of a money amount as well as its currency. The methods `getCurrency` and `getNumber`return `CurrencyUnit`and `NumberValue` do this respectively. The method `NumberValue` extends `java.lang.Number` and as a result it can easily be converted to primitive numerical types (e.g. `int`, `long`, `float`, `double`,`short`,`byte`).

```java
public class RetrieveInformationMethods {

    public  static  void main(String[] args) {

        MonetaryAmount money = Money.of(10, Monetary.getCurrency("BRL"));
        CurrencyUnit currency = money.getCurrency();
        Number number = money.getNumber();

    }
}
```
In the example below we show some methods that `NumberValue` provides. E.g. `getPrecision`,`getScale` 
and `getNumberType`.


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