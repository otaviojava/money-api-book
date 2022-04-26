### Representing Money and Currency with Money-API



As we mentioned before, money is represented with a numeric value and a currency. In order to represent currencies, implementations of JSR-354 have to implement the `CurrencyUnit` interface. The following table summarises the methods available in the `CurrencyUnit` interface. Implementations are required to be immutable and thread-safe.


|Method's name| Description |Example|
| -- | -- | -- |
|```String getCurrencyCode()```|Returns the currency code, to currency that follows the ISO will be returned as three characters.|BRL for Brazilian Real and USD for US Dollars.
|```int getNumericCode()```|Returns the numeric code of the currency, similar to currency code that consists of three digits.|986 for Brazilian Real and 840 for US Dollars.|
|``` int getDefaultFractionDigits()``` |Returns the number of digits normally used by currency.|BRL has two digits|


We will be using  **Moneta**'s implementation in this book, the reference implementation for this specification. There are two ways for creating instances of `CurrencyUnit` in  **Moneta**. The first way is to construct it from the currency code, a three character `String`.


```java
public class CurrencyExample1 {

    public static void main(String[] args) {

        CurrencyUnit currencyUnit = Monetary.getCurrency("BRL");
        String currencyCode = currencyUnit.getCurrencyCode();//BRL
        int numericCurrencyCode = currencyUnit.getNumericCode();//986
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2

    }
}
```

The second option is passing the `Locale` as a parameter, this option could be quite handy in web applications where the `Locale` can be retrieved from the request.

```java

public class CurrencyExample2 {

    public static void main(String[] args) {
        CurrencyUnit currencyUnit = Monetary.getCurrency(Locale.US);
        String currencyCode = currencyUnit.getCurrencyCode();//USD
        int numericCurrencyCode = currencyUnit.getNumericCode();//840
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2
    }
}

```

Now we will discuss how we represent monetary amounts. The `MonetaryAmount` interface is responsible for this. Implementations of this interface are required to be immutable and thread-safe.

||Method|Description|
| -- | -- |
|`<R> R query(MonetaryQuery<R> query)`|Queries the monetary amount|
|`MonetaryAmount with(MonetaryOperator operator)`|Applies operations on the monetary amount.|
|`boolean isGreaterThan(MonetaryAmount amount)`|Returns true if the instance is strictly greater than the value of the passed monetary amount.|
|`boolean   isGreaterThanOrEqualTo(MonetaryAmount amount)`|Returns true if the instance is greater than or equals than the value of the passed monetary amount.|
|`boolean isLessThan(MonetaryAmount amount)`|Returns true if the instance is strictly less than the value of the passed monetary amount.|
|`isLessThanOrEqualTo(MonetaryAmount amt)`|Returns true if the instance is less than or equal to the value of the passed monetary amount.|
|`boolean isEqualTo(MonetaryAmount amount)`|Returns true if the instance is strictly equal to the value of the passed monetary amount.||
|`boolean isNegative()`|Returns true if the instance is negative.|
|`boolean isNegativeOrZero()`|Returns true if the instance is negative or zero.|
|`isPositive()`|Returns true if the instance is positive.|
|`boolean isPositiveOrZero()`|Returns true if the instance is positive or zero.|
|`isZero()`|Returns true if the instance is zero.|
|`MonetaryAmount add(MonetaryAmount amount)`|Adds the value of the instance and the monetary parameter and returns the addition.|
|`MonetaryAmount subtract(MonetaryAmount amount)`|Subtracts the value of the instance and the monetary parameter and returns the subtraction.|
|`MonetaryAmount multiply(Number multiplicand)`|Multiplies the value of the instance by the monetary parameter and returns the multiplication.|
|`MonetaryAmount divide(Number divisor)`|Divides the value of the instance by the monetary parameter and returns the multiplication.|
|`MonetaryAmount remainder(Number divisor)`|Divides the value of the instance by the monetary parameter and returns the remainder.|
|`MonetaryAmount negate()`|Negates the monetary value of the instance.|
|`getCurrency()`| Returns the currency of the monetary amount.|


Within **Moneta** there are three implementations for the `MonetaryAmount` interface:


1. **Money**: The default implementation, it represents the numeric value using BigDecimal.
2. **RoundedMoney**: Similar to **Money**, it represents the numeric value with BigDecimal, however, RoundedMoney allows you to apply a MonetaryOperator on every operation. For example, applying a rounding operation on each arithmetic operation. 
3. **FastMoney**: An implementation which represents the numeric value with a long primitive, it is the fastest implementation on **Moneta**, almost fifteen times faster than the previous implementations. However, it has a precision limitation, the precision is limited to five decimal digits.
