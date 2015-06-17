### Realizando cotización con ExchangeRateProvider

Con money-api es posible realizar la cotización de la moneda, el responsable por esa acción es la interface  ```ExchangeRateProvider```.

```java
public class ExchangeRateProviderExample {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);
        ExchangeRateProvider provider = MonetaryConversions.getExchangeRateProvider(ExchangeRateType.ECB);
        CurrencyConversion currencyConversion = provider.getCurrencyConversion(dollar);
        MonetaryAmount result = currencyConversion.apply(money2);//value on dollar
        MonetaryAmount monetaryAmount = money.add(result);//result on dollar
    }
}
```


Dentro de Moneta existen cuatro implementaciones de ```ExchangeRateProvider```:

* **ECB** implementación que recupera información reciente del Banco Central Europeo.
* **IMF** implementación que recupera las cotizaciones mas recientes del Fondo Internacional Monetario.
* **IMF_HIST** implementación que permite recuperar cotización de una fecha específica a partir del IMF.
* **ECB_HIST90** implementación que recupera los últimos noventa dias del Banco Central Europeo.
* **ECB_HIST** implementación que recupera las cotizaciones desde 1999 del Banco Central Europeo.

