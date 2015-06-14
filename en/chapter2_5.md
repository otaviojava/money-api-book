### Booleans operations

The `MonetaryAmount` has comparatives methods with another `MonetaryAmount`, with this method is possible, for example, verify it's one is greater, lesser, equals, etc.


```java
    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(10, currency);
        MonetaryAmount money2 = Money.of(20, currency);
        boolean equalsToResult = money.isEqualTo(money2);//false
        boolean greaterThan = money.isGreaterThan(money2);//false
        boolean greaterThanOrEqualTo = money.isGreaterThanOrEqualTo(money2);//false
        boolean lessThan = money.isLessThan(money2);//true
        boolean lessThanOrEqualTo = money.isLessThanOrEqualTo(money2);//true
        boolean negative = money.isNegative();//false
        boolean negativeOrZero = money.isNegativeOrZero();//false
        boolean positive = money.isPositive();//true
        boolean positiveOrZero = money.isPositiveOrZero();//true
        boolean zero = money.isZero();//false
    }
}
```


