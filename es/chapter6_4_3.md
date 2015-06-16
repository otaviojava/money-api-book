#### Resumiendo monedas



Otra forma de agrupar dinero es a partir del agrupamiento de `MonetaryAmount`, con este es posible saber el valor mínimo, máximo, suma, media y número de elementos. Para eso es necesario seleccionar la moneda, asi la moneda sea diferente de la seleccionada este será ignorado.


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

**Moneta** tambien tiene soporte para agrupar los resumenes a partir de la moneda.

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


También es posible resumir todos los valores convirtiendo las monedas para la moneda definida en el método, para eso basta seleccionar un `ExchangeRateProvider`.


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