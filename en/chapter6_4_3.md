#### Summarizing the money


Summarizing the money is given the list of money, will return a `MonetarySummaryStatistics`, the object that has informations of the list such minimum and maximum value, average, sum and the number of elements in the list. To do this just need inform the currency, if the `MonetaryAmount` has a different currency, it will be ignored. 



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

The **Moneta** provides also a grouping of `MonetarySummaryStatistics`  from the currencies.

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

Also is possible convert all currencies to just one on `MonetarySummaryStatistics`, to do this just inform an `ExchangeRateProvider`.

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
                .collect(MonetaryFunctions.summarizingMonetary(dollar, provider));

        MonetaryAmount min = summary.getMin();//USD 2.831248
        MonetaryAmount max = summary.getMax();//USD 25
        MonetaryAmount average = summary.getAverage();//USD 10.195416
        long count = summary.getCount();//5
        MonetaryAmount sum = summary.getSum();//50.97708
        
    }
}
```