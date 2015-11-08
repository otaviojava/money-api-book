#### Summarizing Money

A utility that provides some useful statistics for a given stream of monetary values. This results into an object of type `MonetarySummaryStatistics`. This object contains some statistical information such as the minimum and maximum values, average, sum, and the number of elements in the stream. A currency must be provided and amounts with different currencies will be ignored. Here's an example.


```java
public class AggregateSummaringMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(25, dollar);

        MonetarySummaryStatistics summary = Stream.of(money, money2, money3, money4, money5)
                .collect(MonetaryFunctions.summarizingMonetary(dollar));

        MonetaryAmount min = summary.getMin();//USD 10
        MonetaryAmount max = summary.getMax();//USD 25
        MonetaryAmount average = summary.getAverage();//USD 15
        long count = summary.getCount();//3
        MonetaryAmount sum = summary.getSum();//USD 45
        
    }
}
```
 **Moneta** provides grouping of `MonetarySummaryStatistics`  from the currencies.

```java
public class AggregateGroupSummaringMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(25, dollar);

        GroupMonetarySummaryStatistics grouped = Stream.of(money, money2, money3, money4, money5)
        .collect(MonetaryFunctions.groupBySummarizingMonetary());
        Map<CurrencyUnit, MonetarySummaryStatistics> mapSummary = grouped.get();
        MonetarySummaryStatistics summary = mapSummary.get(dollar);

        MonetaryAmount min = summary.getMin();//USD 10
        MonetaryAmount max = summary.getMax();//USD 25
        MonetaryAmount average = summary.getAverage();//USD 15
        long count = summary.getCount();//3
        MonetaryAmount sum = summary.getSum();//USD 45

    }
}
```
Currency conversions can be applied to `MonetarySummaryStatistics`if an `ExchangeRateProvider`is provided. 

```java
public class AggregateSummaringExchangeRateMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF);

        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(25, dollar);

        MonetarySummaryStatistics summary = Stream.of(money, money2, money3, money4, money5)
                .collect(ConversionOperators.summarizingMonetary(dollar, provider));

        MonetaryAmount min = summary.getMin();//USD 2.831248
        MonetaryAmount max = summary.getMax();//USD 25
        MonetaryAmount average = summary.getAverage();//USD 10.195416
        long count = summary.getCount();//5
        MonetaryAmount sum = summary.getSum();//50.97708
        
    }
}
```