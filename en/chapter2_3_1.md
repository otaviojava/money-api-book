#### RoundedMoney Factory methods

In the following example, we show some static factory methods that could be used instantiate  `RoundedMoney`.  One of the overloads expects a `MonetaryOperator` which is used as a “*rounding agent*”. Keep in mind that rounding is what`RoundedMoney` provides.

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