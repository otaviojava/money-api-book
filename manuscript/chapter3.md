## Manipulando e extraindo informação do MonetaryAmount


Além de realizar operações básicas como as operações aritméticas, algumas vezes é necessário extrair ou converter informações do dinheiro, por exemplo, extrair a menor parte do dinheiro, obter apenas os centavos, ou a maior parte do dinheiro. Com esse intuito existe as interfaces ```MonetaryOperator``` e ```MonetaryQuery```. Ambas as interfaces possuem apenas um método para ser implementados, ou seja, são interfaces funcionais.


```java

@FunctionalInterface
public interface MonetaryOperator{

    MonetaryAmount apply(MonetaryAmount amount);
}


@FunctionalInterface
public interface MonetaryQuery<R>{

    R queryFrom(MonetaryAmount amount);
}

```

### MonetaryOperator


O ```MonetaryOperator``` é uma interface funcional que recebe um ```MonetaryAmount``` e retorna um ```MonetaryAmount```. Essa interface é muito discutida ao se falar sobre a implementação do ```RoundedMoney``` que a utiliza como agente arredondador. Com essa interface é possível realizar operações de arredondamento, retornar parte do dinheiro, o dobro do valor etc. 

```java
public class MonetaryOperatorExamples {

    public  static void main(String[] args) {
        MonetaryOperator doubleOperator = m -> m.multiply(2);
        MonetaryOperator halfOperator = m -> m.divide(2);
        MonetaryOperator operator = m -> {
            if(m.isPositive()){
                return m.multiply(2);
            }
            return m.divide(2);
        };
    }
}
```

Para o executá-lo basta chamar o método apply da interface ou chamar o método **with** dentro do ```MonetaryAmount```.


```java
public class HelloMonetaryOperator {

    public  static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, currency);
        MonetaryOperator doubleOperator = m -> m.multiply(2);
        MonetaryAmount result = doubleOperator.apply(money);//BRL 20.00000
        MonetaryAmount result2 = result.with(doubleOperator);//BRL 40.00000
    }
}
```


O moneta traz por padrão algumas implementações de `MonetaryOperator`, a classe `MonetaryOperators`. Ela é uma classe utilitária que traz algumas funcionalidades importantes e algumas vezes muito corriqueira dentro da vida de um desenvolvedor Java, como:



* **reciprocal()** Retorna o dinheiro como reciprocal, multiplicando pelo valor inverso (1/n).
* **permil(Number number)** retorna o valor permil do dinheiro, por exemplo, **permil(10)** de `EUR 2.35` retornará EUR `0.0235`.
* **percent(Number number)** retorna o percentual de um dinheiro, por exemplo, **percent(10)** de `EUR 200.00` retornará `EUR 20.00`.
* **minorPart()** retorna o valor que se encontra na direita da vírgula, por exemplo, a menor parte de `EUR 2.35` é ```EUR 0.35```.
* **majorPart()** retorna o valor inteiro do dinheiro, por exemplo, a maior parte de `EUR 2.35` é `EUR 2`.
* **rounding()** realiza o processo de arredondamento do dinheiro, para saber o número de casas após a vírgula é recuperado a informação do método getDefaultFractionDigits() da interface CurrencyUnit.
* **exchange(CurrencyUnit currency)** dado um dinheiro esse operador realizará a troca da moeda, ou seja, ele apenas vai trocar mudar a moeda não levando em consideração a sua cotação, por exemplo, `EUR 2.35` **exchange('BRL')** retornará `BRL 2.35`. Este método está na classe `ConversionOperators`.

```java

public class MonetaryOperatorsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        CurrencyUnit dollar = Monetary.getCurrency(Locale.US);
        
        MonetaryAmount money = Money.of(120.231, currency);
        
        MonetaryAmount majorPartResult = money.with(MonetaryOperators.majorPart());//BRL 120
        MonetaryAmount minorPartResult = money.with(MonetaryOperators.minorPart());//BRL 0.231
        MonetaryAmount percentResult = money.with(MonetaryOperators.percent(20));//BRL 24.0462
        MonetaryAmount permilResult = money.with(MonetaryOperators.permil(100));//BRL 12.0231
        MonetaryAmount roundingResult = money.with(MonetaryOperators.rounding());//BRL 120.23
        MonetaryAmount resultExchange = money.with(ConversionOperators.exchange(dollar));//USD 120.231
    }
}
```

### MonetaryQuery


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

### MonetaryQuery vs MonetaryOperator 


Mas com o ```MonetaryQuery``` é possível também retornar ```MonetaryAmount``` e assim temos o mesmo resultado que um ```MonetaryOperator```, assim qual é o objetivo de ter duas interfaces? O Objetivo de terem as duas interfaces é por questão de nomenclatura e padronização. ```MonetaryQuery``` tem o objetivo extrair e buscar informações dentro do ```MonetaryAmount```, já o ```MonetaryOperator``` tem o objetivo de realizar operações com o dinheiro.

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {

        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
