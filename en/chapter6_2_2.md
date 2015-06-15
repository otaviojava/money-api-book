#### Maximum and minimum value


**Moneta** supports the maximum and minimum reduction, so given a list will return the biggest or the lowest value.


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

If a list has different currencies is possible to do exchange rate before the comparison. If don't do that, the comparison with different currencies will launch an exception.


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