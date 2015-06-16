#### Valor mínimo y valor máximo

Es posible realizar la reducción por el valor máximo y mínimo, por ejemplo, dado una lista es posible retornar el mayor o el menor valor de la lista.


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

Si es realizado la operación de mínimo y máximo con monedas diferentes, sucederá una excepción. Es posible pasar un `ExchangeRateProvider` para realizar la conversión y entonces la comparación.


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