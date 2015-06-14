### Creation Methods


To create a `MonetaryAmount` instance, all implementations follow the same standard for nomenclature, with a little exception on `RoundedMoney` once it can receive a `MonetaryOperator` to work as rounding agent on each operation. Listing the most important methods have:

* The method “**of**” with a number and the `CurrencyUnit`.
The method “**zero**” with a `CurrencyUnit`.
The method “**ofMinor**” with a long and a `CurrencyUnit`, this `long` will take as cents and will converted considering the currency fraction digits, for example, 200 cents of dollar will two dollars.



```java
public class MethodsCreationMoney {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(BigDecimal.TEN, currency); //BRL 10
        MonetaryAmount zero = Money.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = Money.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = Money.ofMinor(currency, 100_00);//BRL 10
    }
}
```


```java
public class MethodsCreationsFastMoney {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(BigDecimal.TEN, currency); //BRL 10
        MonetaryAmount zero = FastMoney.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = FastMoney.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = FastMoney.ofMinor(currency, 100_00);//BRL 10
    }
}
```


```java
public class MethodsCreationRoundedMoney {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = RoundedMoney.of(BigDecimal.TEN, currency); //BRL 10
        MonetaryAmount zero = RoundedMoney.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = RoundedMoney.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = RoundedMoney.ofMinor(currency, 100_00);//BRL 10
    }
}
```
 




