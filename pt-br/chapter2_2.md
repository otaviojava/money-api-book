## Operações Aritméticas


Com o ```MonetaryAmount``` é possível realizar operações como subtração e soma, salientando que as implementações são imutáveis, ou seja, o resultado resultará em uma nova instância. Ao realizar operações que recebem um ```MonetaryAmount``` o resultado será também um ```MonetaryAmount```, mas da implementação da instância que chamou o método.

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

Para as operações que de multiplicação, divisão e resto é necessário passar um parâmetro do tipo ```Number```.

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

Também é possível realizar operações de sinais com o ```MonetaryAmount```.



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