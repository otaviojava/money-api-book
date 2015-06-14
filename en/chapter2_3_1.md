#### Creations methods to RoundedMoney


Beyond the same creation methods, the `RoundedMoney` class has others ways, mainly adding the `MonetaryOperator` to be used as “*rounding agent*”, just to remember, this rounding is the only reason to this implementation exists, if doesn't need it, the recommendation is to use another implementation.


```java
public class RoundedMoneyCreation2 {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = RoundedMoney.of(BigDecimal.TEN, currency, MonetaryOperators.rounding()); //BRL 10
        MonetaryAmount zero = RoundedMoney.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = RoundedMoney.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = RoundedMoney.ofMinor(currency, 100_00);//BRL 10
    }
}
```