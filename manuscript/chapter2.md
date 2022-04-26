## What has been done? Knowing the API


In the last chapter we discussed the advantages of using a type class to represent money instead of a primitive value. But has anyone done this before? We do not want to “*re-invent the wheel*” and spend a lot of time and effort only to have the same problems that an advanced developer has already had. A famous phrase from Clarice Lispector says “Who walks alone might even get faster, but one that is accompanied surely goes further.” In other words, the best strategy is to join with more people and communities and in this way it is possible to share experiences, not repeat the same errors, and contribute to these tools to make them more mature and stronger. Relying on a single library or vendor is not very flexible, and since fragmentation of solutions can lead to the same mistakes being made many times, the obvious solution is to create a standard way of solving the money problem. With this goal in mind, the specification, **JSR 354** was born. 


Once the interface is defined, each company, community, or group of developers can chose their favorite solution or can change it to use another implementation of their application. To do so, it just needs the money representation that implements this API. This behavior is similar to Java Persistance API. If the developers follow or use the JDBC's interfaces, it is possible to change the database by changing the driver and the implementation. Everything will then run normally.

Using the money's approach, the money has two parts, the part of value is the number quantity. This is represented numerically, however with the number value alone, it's not possible do much. It is necessary to also use the coin. The coin represents the “system of money” in trivial use, specially within a country. This way the Brazilian real, dollar and euro are a kind of currency.

### Source code and Installation

The API provides a reference implementation that will be the base to all future implementations. On JSR 354, Money-Api, the reference implementation is **Moneta**. For more information about the project see [https://github.com/JavaMoney/jsr354-ri](https://github.com/JavaMoney/jsr354-ri).

---

####Installing the Project

You can either add it to your project as a dependency from the Maven repository or install it from source. 

The Maven repo can be found here:
[http://mvnrepository.com/artifact/org.javamoney/moneta](http://mvnrepository.com/artifact/org.javamoney/moneta).



---


#####Adding the dependencies 

Maven

```xml
        <dependency>
            <groupId>org.javamoney</groupId>
            <artifactId>moneta</artifactId>
            <version>moneta_version</version>
        </dependency>
```

Gradle:
```
'org.javamoney:moneta:moneta_version'
```

Ivy:

```
<dependency org="org.javamoney" name="moneta" rev="moneta_version"/>
```

Now you are ready to use the API in your project.



---


#####Installing from Source

Another option is to install the project from source. This requires having Git and Maven installed as well as Java 8.


Clone the source of javamoney-parent using the following command:

```
git clone https://github.com/JavaMoney/javamoney-parent.git
```

Switch to the containing folder:

```
cd javamoney-parent
```

Compile the source and install it, the tests will be skipped in this case:

```
mvn clean install -Dmaven.test.skip
```

The next step will be to install **Moneta** from source.

Clone Moneta's source to your local machine:

```
git clone git@github.com:JavaMoney/jsr354-ri.git
```

Switch to the containing folder:
```
cd jsr354-ri
```

Compile the source and install it, the tests will be skipped in this case:

```
mvn clean install -Dmaven.test.skip
```

You're Ready! The source code was compiled and installed on a local repository on your machine. In this book we will be using the master from the Github repository, so we recommend installing locally. The source for the samples used in this book is available on Github: [https://github.com/otaviojava/money-api-book-samples](https://github.com/otaviojava/money-api-book-samples).



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

### Creation Methods


To create a `MonetaryAmount` instance, all implementations follow the same standard for nomenclature, with one 
exception for `RoundedMoney`. `RoundedMoney` can receive a `MonetaryOperator` to work as a rounding agent on each operation. The most important methods here are:

* The method “**of**” with a number and the `CurrencyUnit`.
* The method “**zero**” with a `CurrencyUnit`.
* The method “**ofMinor**” with a `long` and a `CurrencyUnit`. The `long` will take the cents and convert them using the currency fraction digits, for example, 200 cents becomes 2 dollars.



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
 

#### RoundedMoney Factory methods


In the following example, we show some static factory methods that could be used to instantiate `RoundedMoney`. One of
 the overloads expects a `MonetaryOperator` that is used as a “*rounding agent*”. Keep in mind that rounding is what`RoundedMoney` provides.


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

### Arithmetic Operations


Implementations of `MonetaryAmount` are required to be immutable and as such those operations will create new instances holding the result when applied without mutating the original instance.


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

To do multiplication or division, a remainder operation is necessary to pass a `Number` instance parameter.

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

Also, it is possible to do an operation just using signs on `MonetaryAmount`.

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

### Boolean Operations

`MonetaryAmount` provides equality, relational, and other convenience methods as shown in the following examples.


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