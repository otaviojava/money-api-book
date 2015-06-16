#### Sumatoria

La suma o la reducción por la suma es definido en: Dado una lista de `MonetaryAmount` este retornará un elemento con la sumatoria de esos valores.


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

Resaltando algo, en caso tenga una moneda diferente en la sumatoria este retornará una excepción.

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

En caso sea necesario sumar y convertir para una específica moneda, Moneta da soporte para eso, basta proporcionar una implementación de `ExchangeRateProvider` y tambien la moneda en la que todos los valores serán convertidos.

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
