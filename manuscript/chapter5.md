## Cotação


A internacionalização é um conceito aplicado em diversas áreas, basicamente, é uma troca de cultura, economia e política em diversos lugares do mundo. Com a internacionalização é possível, por exemplo, saber o que acontece do outro lado mundo de maneira instantânea, conhecer lugares e culturas sem sair de casa além de adquirir produtos. É muito comum exportar e importar diversas coisas, produtos, serviços, assinaturas, etc. Seguindo a tendência mundial os softwares também precisam estar preparados para uma arquitetura internacionalizada, ou seja, permitir a interação de usuário em diversos pontos do mundo. Com o dinheiro não é diferente, é necessário estar pronto para isso.

Falando de internacionalização e dinheiro é natural que precise interagir com diversas moedas, mas o que acontece ao realizar operações com moedas diferentes?


```java
public class MistakeExample {


    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);
        MonetaryAmount result = money.add(money2);//javax.money.MonetaryException: Currency mismatch: USD/BRL
    }
}
```


Utilizando o money-api, ele gerará uma exceção afirmando que existe um erro ao tentar somar dinheiro com moedas diferentes, nesse caso real com dólar. Essa exceção foi lançada de forma correta uma vez que não necessariamente um dólar equivale a um real. Para realiza tal operação de somatório entre moedas diferentes é necessário primeiro realizar a cotação da moeda. A taxa de câmbio é a relação das moedas entre dois países que resulta no preço de uma delas com relação a outra. 

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

### Realizando a cotação a partir de uma data específica

Em alguns momentos da aplicação é importante saber não apenas o valor da cotação atual, mas a partir de uma data específica, por exemplo, ao se alugar um hotel normalmente o valor de cotação é dado a partir da confirmação da reserva ou no caso do cartão de crédito o valor da cotação é definido apenas no fechamento da fatura. Com o moneta é possível realizar tal busca a partir de uma data específica para isso é utilizado a  classe ```ConversionQuery``` com ela é possível realizar buscas de datas diferentes ou num range de datas. A representação de data aceita é a classe ```LocalDate```.


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

Caso a data especificada não seja encontrada será retornada uma exceção, por exemplo, não será possível recuperar a cotação do dia 9 de janeiro de 2011, uma vez que essa data foi em um domingo e a grande maioria dos provedores de cotação não trabalham nesse dia.

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


Uma possível solução para esse problema é passar um range de datas, assim a implementação vai procurar algumas das datas, caso não encontre nenhuma delas lançará uma exceção, vale salientar que a implementação buscará a partir da ordem que foi definida.

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

### Ignorando a cotação

Em alguns momentos é necessário realizar uma troca de moeda sem realizar cotação, para isso existe o método **exchange** dentro do ```ConversionOperators```.

```java
public class ExchangeRateProviderExample5 {

    public static void main(String[] args) {

        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);


        MonetaryOperator operator = ConversionOperators.exchange(dollar);
        MonetaryAmount result = money2.with(operator).add(money);//USD 20.00000 ignoring currency
    }
}
```