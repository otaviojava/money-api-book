### Realizando la cotización a partir de una fecha especifica

En algunos momentos de la aplicación es importante saber no solo el valor de la cotización actual, sino a partir de una fecha especifica, por ejemplo, al alquilar un hotel normalmente el valor de la cotización es dado a partir de la confirmación de la reserva o en el caso de la tarjeta de crédito el valor de la cotización es definido solo en el cierre de la factura. Con Moneta es posible realizar tal busqueda a partir de una fecha especifica para eso se utiliza la clase ```ConversionQuery``` con esta es posible realizar busquedas de fechas diferentes o en un rango de fechas. La representación aceptada de fecha es la clase ```LocalDate```.


```java
public class ExchangeRateProviderExample2 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        LocalDate localDate = Year.of(2009).atMonth(Month.JANUARY).atDay(9);

        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF_HIST);
        ConversionQuery query = ConversionQueryBuilder.of().setTermCurrency(dollar).set(localDate).build();

        CurrencyConversion currencyConversion = provider.getCurrencyConversion(query);

        MonetaryAmount result = currencyConversion.apply(money2);
        MonetaryAmount monetaryAmount = money.add(result);
        System.out.println(monetaryAmount);
    }
}
```

Si la fecha especificada no sea encontrada será retornada una excepción, por ejemplo, no será posible recuperar la cotización del dia 9 de enero de 2011, ya que esa fecha fue en un domingo y la gran mayoria de los proveedores de cotización no trabajan en ese dia.

```java
public class ExchangeRateProviderExample3 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        LocalDate localDate = Year.of(2011).atMonth(Month.JANUARY).atDay(9);

        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF_HIST);
        ConversionQuery query = ConversionQueryBuilder.of().setTermCurrency(dollar).set(localDate).build();

        CurrencyConversion currencyConversion = provider.getCurrencyConversion(query);

        MonetaryAmount result = currencyConversion.apply(money2);//javax.money.MonetaryException: There is not exchange on day 2011-01-09 to rate to  rate on IFMRateProvider.


    }
}
```


Una posible solución para este problema es pasar un rango de fechas, asi la implementación va probar alguna de las fechas, si no encuentra ninguna de ellas lanzará una excepción, vale recalcar que la implementación buscará a partir de la orden de la que fue definida.

```java
public class ExchangeRateProviderExample4 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        LocalDate localDate = Year.of(2011).atMonth(Month.JANUARY).atDay(9);
        LocalDate[] localDates = Stream.of(localDate, localDate.minusDays(1L), localDate.minusDays(2L),
                localDate.minusDays(3L)).sorted(Comparator.<LocalDate>naturalOrder().reversed()).toArray(LocalDate[]::new);
        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF_HIST);
        ConversionQuery query = ConversionQueryBuilder.of().setTermCurrency(dollar).set(localDates).build();

        CurrencyConversion currencyConversion = provider.getCurrencyConversion(query);

        MonetaryAmount result = currencyConversion.apply(money2);
        MonetaryAmount sum = money.add(result);

    }
}
```