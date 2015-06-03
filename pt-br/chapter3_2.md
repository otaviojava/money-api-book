### MonetaryQuery: 


O ```MonetaryQuery``` semelhante ao ```MonetaryOperator``` é uma interface funcional que recebe um ```MonetaryOperator```, a sua diferença está no retorno, ele pode retornar qualquer tipo de objeto a partir do *generics*. Com o ```MonateryQuery``` é possível recuperar algumas informações no ```MonetaryAmount```, por exemplo, o código da moeda, apenas o número no formato long ou em BigDecimal.

```java
public class MonetaryQueryExamples {

    public static void main(String[] args) {
        MonetaryQuery<Long> longQuery = m -> m.getNumber().longValue();
        MonetaryQuery<String> currencyCodeQuery = m -> m.getCurrency().getCurrencyCode();
        MonetaryQuery<Integer> fractionDigits = m -> m.getCurrency().getDefaultFractionDigits();
        

    }
}
```

Para o executá-lo basta chamar o método apply da interface ou chamar o método “with” dentro do MonetaryAmount.


```java
public class HelloMonetaryQuery {

    public  static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, currency);
        MonetaryQuery<String> currencyCodeQuery = m -> m.getCurrency().getCurrencyCode();
        String result = currencyCodeQuery.queryFrom(money);//BRL
        String result2 = money.query(currencyCodeQuery);//BRL
    }
}
```


O moneta traz por padrão algumas implementações de ```MonetaryQuery```, a classe ```MonetaryQueries```. Ela é uma classe utilitária que traz algumas funcionalidades importantes e algumas vezes muito corriqueira dentro da vida de um desenvolvedor Java, como:

* **MonetaryQuery<Long> extractMajorPart()** recupera a maior parte de um dinheiro, por exemplo, `EUR 2.35` retornará `2`.
* **MonetaryQuery<Long> convertMinorPart()** recupera o valor monetário, o convertendo para a menor parte, centavos de um dinheiro, por exemplo, `USD 2.35` será retornado o `235`.


```java
public class MonetaryQueriesExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency(Locale.US);

        MonetaryAmount money = Money.of(120.23, currency);
        Long moneyInCents = money.query(MonetaryQueries.convertMinorPart());//12023
        Long majorPart = money.query(MonetaryQueries.extractMajorPart());//120
        Long minorPart = money.query(MonetaryQueries.extractMinorPart());//23
    }
}
```