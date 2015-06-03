### Realizando cotação com ExchangeRateProvider

Com o money-api é possível realizar a cotação da moeda, o responsável por essa ação é a interface ```ExchangeRateProvider```.

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


Dentro do moneta existem quatro implementações de ```ExchangeRateProvider```:

* **ECB** implementação que recupera informação recente do Banco Central Europeu.
* **IMF** implementação que recupera as cotações mais recentes do Fundo Internacional Monetário.
* **IMF_HIST** implementação que permite recuperar cotação de uma data específica a partir do IMF.
* **ECB_HIST90** implementação que recupera os últimos noventa dias do Banco Central Europeu.
* **ECB_HIST** implementação que recupera as cotações desde 1999 do Banco Central Europeu.

