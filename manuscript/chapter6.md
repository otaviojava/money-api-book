## Working with Streams

This API was developed under Java 8, and hence it's uses Java 8 functionality. For instance, `java.time.LocalDate` is used to retrieve the exchange rate at a specific time. **Moneta** provides some higher order functions in the `MonetaryFunctions` class that help you when working with Streams.

### Sorting a List of Monetary Values

`MonetaryFunctions` provides functions for sorting monetary values by currencies or numerical values, and uses exchange rates for applying conversions. Sorting can be by ascending or descending order. 

#### Sorting by Currency
When you sort by currency, **Moneta** sorts the list based on the lexicographical order of the currency code. For example, a list of money with the currencies `USD`, `EUR`, `BRL` will be sorted to `BRL`, `EUR` and `USD` when sorted in ascending order or `USD`, `EUR`, `BRL` when sorted in descending order. Here's an example. 


```java
public class SortMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnit()).collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnitDesc()).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```

#### Sorting by the Numerical Value

When you sort by the numerical value, the currency will be ignored and only the numerical value will be used in the comparisons. Note that this sorting will not factor in exchange rates. In other words, ten Brazilian Real will be considered equal to ten US Dollars. Sorting can be in ascending or descending order. Here's an example.

```java
public class SortMonetaryAmountNumber {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortNumber()).collect(Collectors.toList());//[EUR 9, USD 10, BRL 11]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortNumberDesc()).collect(Collectors.toList());//[BRL 11, USD 10, EUR 9]
    }
}
```

#### Sorting by the Exchange Rate

Sorting can be applied on the exchange rate, but this requires providing an implementation of `ExchangeRateProvider`. For example given a list of `[10 USD,11 BRL,9 EUR]`, sorting them by the ascending order will result in `[11 BRL,10 USD,9 EUR]` because BRL has a lower exchange rate than USD, and USD has a lower exchange rate than EUR.


```java
public class SortMonetaryAmountExchange {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        ExchangeRateProvider provider =
                MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF);

    
        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValuable(provider))
                .collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]

        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValuableDesc(provider)).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```

#### Multiple Sorters 

The `Comparator.thenComparing` method can be used to chain multiple sorting comparators. Essentially, the secondary comparator will be applied when two values are equal according to the first comparator. This method was added in Java 8. Here's an example.

```java
public class SortMixMonetaryAmountNumberCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(10, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, real);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortNumber().thenComparing(MonetaryFunctions.sortCurrencyUnit()))
                .collect(Collectors.toList());//[USD 8, BRL 9, BRL 10, EUR 10, USD 10]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortNumberDesc().thenComparing(MonetaryFunctions.sortCurrencyUnitDesc()))
                .collect(Collectors.toList());//[USD 10, EUR 10, BRL 10, BRL 9, USD 8]
        //using currency first
        List<MonetaryAmount> resultCurrencyAsc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnit().thenComparing(MonetaryFunctions.sortNumber()))
                .collect(Collectors.toList());//[BRL 9, BRL 10, EUR 10, USD 8, USD 10]
        List<MonetaryAmount> resultCurrencyDesc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnitDesc().thenComparing(MonetaryFunctions.sortNumberDesc()))
                .collect(Collectors.toList());//[USD 10, USD 8, EUR 10, BRL 10, BRL 9]

    }
}
```

### Reduction methods

Reduce, also known as `fold`, is a higher order function that, when applied on a stream, results in a value or none. 
For example, sum, average, min, and max.

#### Sum
Sum is a reduction method. Given a stream of `MonetaryAmount`, it returns the sum of its elements. Here's an example. 

```java
public class ReduceSumMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");

        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);

        Optional<MonetaryAmount> result = Stream.of(money, money2, money3, money4, money5).reduce(MonetaryFunctions.sum());
        result.ifPresent(System.out::println);//USD 47

    }
}
```
Notice that the operation will throw an exception if applied on a stream of money with different currencies as shown below.   
```java
public class ReduceSumMonetaryAmountError {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, real);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);

        Optional<MonetaryAmount> result = Stream.of(money, money2, money3, money4, money5).reduce(
                MonetaryFunctions.sum());//javax.money.MonetaryException: Currency mismatch: BRL/USD
        
    }
}
```
In **Moneta**, you can provide an implementation of `ExchangeRateProvider` in order to perform currency conversions for monetary values  with different currencies as shown below. 
```java
public class ReduceSumMonetaryAmountExchange {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF);

        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, real);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);

        Optional<MonetaryAmount> result = Stream.of(money, money2, money3, money4, money5).reduce(
                MonetaryFunctions.sum(provider, dollar));//javax.money.MonetaryException: Currency mismatch: BRL/USD
        result.ifPresent(System.out::println);//money converted in dollar
        
    }
}
```

#### Maximum and Minimum

**Moneta** provides maximum and minimum reductions. Given a stream of monetary values, you could easily get the maximum or minimum values. Here's an example. 

```java
public class ReduceMaxMinMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);

        Optional<MonetaryAmount> max = Stream.of(money, money2, money3, money4, money5).reduce(
                MonetaryFunctions.max());//javax.money.MonetaryException: Currency mismatch: BRL/USD
        max.ifPresent(System.out::println);//USD 10

        Optional<MonetaryAmount> min = Stream.of(money, money2, money3, money4, money5).reduce(
                MonetaryFunctions.min());
        min.ifPresent(System.out::println);//USD 8
        
    }
}
```
An exception will be thrown if a currency mismatch occurs. To avoid that, an `ExchangeRateProvider` must be provided as shown below. 

```java
public class ReduceMaxMinMonetaryAmountExchange {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF);

        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, euro);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, euro);
        MonetaryAmount money5 = Money.of(8, dollar);

        Optional<MonetaryAmount> max = Stream.of(money, money2, money3, money4, money5).reduce(
                MonetaryFunctions.max(provider));//javax.money.MonetaryException: Currency mismatch: BRL/USD
        max.ifPresent(System.out::println);//EUR 10

        Optional<MonetaryAmount> min = Stream.of(money, money2, money3, money4, money5).reduce(
                MonetaryFunctions.min(provider));
        min.ifPresent(System.out::println);//USD 8
        
    }
}
```

### Predicates

A predicate is a higher order function that when applied to an element in the stream, returns true if the condition holds, or false otherwise. Predicates can be used for filtering (ex: filtering by currency). Here's an example.


```java
public class BooleanMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Stream<MonetaryAmount> justDollars = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar));

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//true
    }
}
```

#### Currency Predicates

**Moneta** Provides currency predicates that help in filtering based on the currency. These predicates could be inclusive or exclusive as we will show through the examples.


* `isCurrency(CurrencyUnit... currencies)`: Returns true if the `MonetaryAmount` has one of these currencies.   
* `filterByExcludingCurrency(CurrencyUnit... currencies)`: Returns true if `MonetaryAmount` doesn't have the passed currencies.


```java
public class PredicateMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        CurrencyUnit euro = Monetary.getCurrency("EUR");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> justDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar)).collect(Collectors.toList());//[USD 10, USD 10, USD 8]

        boolean anyDollar = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allDollar = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//false

        List<MonetaryAmount> notDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.filterByExcludingCurrency(dollar)).collect(Collectors.toList());//[BRL 10, BRL 9]

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true


    }
}
```
#### Currency Predicates

**Moneta** Provides currency predicates that help in filtering based on the currency. These predicates could be inclusive or exclusive as we will show through the examples.


* `isCurrency(CurrencyUnit... currencies)`: Returns true if the `MonetaryAmount` has one of these currencies.   
* `filterByExcludingCurrency(CurrencyUnit... currencies)`: Returns true if `MonetaryAmount` doesn't have the passed currencies.


```java
public class PredicateMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        CurrencyUnit euro = Monetary.getCurrency("EUR");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> justDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar)).collect(Collectors.toList());//[USD 10, USD 10, USD 8]

        boolean anyDollar = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allDollar = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//false

        List<MonetaryAmount> notDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.filterByExcludingCurrency(dollar)).collect(Collectors.toList());//[BRL 10, BRL 9]

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true


    }
}
```

#### Chaining Predicates


Predicates are quite powerful. They provide convenient methods like **negate**, **and** and **or** and hence it is easy to chain them. Here's an example.

```java
public class PredicateMonetaryAmountMix {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);
        MonetaryAmount money6 = Money.of(8, dollar);

		List<MonetaryAmount> greaterThanZeroAndIsReal = Stream
				.of(money, money2, money3, money4, money5, money6)
				.filter(MonetaryFunctions.isGreaterThan(Money.zero(dollar))
						.and(MonetaryFunctions.isCurrency(real)))
				.collect(Collectors.toList());//[]

		List<MonetaryAmount> greaterThanZeroOrIsReal = Stream
				.of(money, money2, money3, money4, money5, money6)
				.filter(MonetaryFunctions.isGreaterThan(Money.zero(dollar))
						.or(MonetaryFunctions.isCurrency(real)))
				.collect(Collectors.toList());//[USD 10, USD 10, USD 10, USD 9, USD 8, USD 8]

		List<MonetaryAmount> notGreaterThan = Stream
				.of(money, money2, money3, money4, money5, money6)
				.distinct()
				.filter(MonetaryFunctions.isGreaterThan(Money.of(9, dollar))
						.negate())
				.collect(Collectors.toList());//[USD 9, USD 8]

    }
}
```
