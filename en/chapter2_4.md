### Arithmetic Operations

`MonetaryAmount` provides arithmetic operations such as sum and subtract. 
Implementations of `MonetaryAmount` are required to be immutable and hence these operations will create new instances holding the result when applied without mutating the instance. 

```java
public class ArithmeticOperations {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(BigDecimal.TEN, currency);
        MonetaryAmount money2 = FastMoney.of(BigDecimal.TEN, currency);
        MonetaryAmount addResult = money.add(money2);//BRL 20 Money implementation
        MonetaryAmount subtractResult = money2.subtract(addResult);//BRL -10 FastMoney implementation
    }
}
```

To do multiplication, division and remain operation is necessary to pass a `Number` instance parameter.

```java
public class ArithmeticOperations2 {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(100, currency);
        Number number = 20;
        MonetaryAmount divideResult = money.divide(number);//BRL 5
        MonetaryAmount remainderResult = money.remainder(30);//BRL 10
        MonetaryAmount resultMultiply = money.multiply(5);//BRL 500
    }
}
```

Also is possible do operation just using signal on `MonetaryAmount`.



```java
public class ArithmeticOperations3 {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(100, currency);
        MonetaryAmount negateResult = money.negate();//BRL -100
        MonetaryAmount plusResult = money.plus();//BRL 100
    }
}
```