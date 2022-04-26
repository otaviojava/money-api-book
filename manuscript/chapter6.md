## Trabalhando com Streams

Uma vez que a API nasceu após o Java 8, é natural que ele tenha suporte a alguns recursos do Java 8, como já vimos no capítulo anterior, a busca de uma cotação é data a partir de um ```LocalDate```, sem falar no suporte a nova estrutura de dados que melhorou a experiência em trabalhar com listas, o **Stream**. Trabalhar com coleções é muito importante e comum, por exemplo, uma lista de produtos que serão cobrados é natural que seja impresso o valor total dessa compra. Para trabalhar com Stream a referência de implementação tem a classe ```MonetaryFunctions```.

### Ordenando uma lista monetária


Dentro da classe `MonetaryFunctions` é possível ordenar pela moeda, pelo valor numérico apenas, além da valiosidade de um dinheiro, levando em consideração a cotação da moeda, de forma ascendente e descendente. 

#### Realizando ordenação com moeda

No caso da ordenação da moeda é levado em consideração o código da moeda. Por exemplo, uma lista com as moedas `USD`, `EUR`, `BRL` retornará `BRL`, `EUR` e `USD` de forma ascendente e USD, EUR, BRL de forma decrescente.

```java
public class SortMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnit()).collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnitDesc()).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```

#### Realizando ordenação com o valor numérico

A ordenação pelo valor numérico ignora a moeda e ordena apenas levando em consideração o valor monetário, vale salientar, que essa ordenação não realiza cotação de valores, em outras palavras, o valor de dez reais terá o mesmo valor que dez dólares. Também é possível retornar de forma ascendente e descendente.

```java
public class SortMonetaryAmountNumber {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortNumber()).collect(Collectors.toList());//[EUR 9, USD 10, BRL 11]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortNumberDesc()).collect(Collectors.toList());//[BRL 11, USD 10, EUR 9]
    }
}
```

#### Realizando ordenação levando em consideração a cotação


Também é possível realizar uma ordenação de forma crescente e decrescente levanto em consideração a cotação da moeda. Para isso basta passar uma implementação de `ExchangeRateProvider`. Por exemplo, dado uma lista com dez dólares, onze reais e nove euros, retornará de forma ascendente o valor de onze reais, dez dólares e nove euros levando em consideração que pela cotação o dólar é mais valioso que o real e menos que valioso que o euro.


```java
public class SortMonetaryAmountExchange {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(9, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(11, real);

        ExchangeRateProvider provider =
                MonetaryConversions.getExchangeRateProvider(ExchangeRateType.IMF);

    
        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValuable(provider))
                .collect(Collectors.toList());//[BRL 11, EUR 9, USD 10]

        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3)
                .sorted(MonetaryFunctions
                        .sortValuableDesc(provider)).collect(Collectors.toList());//[USD 10, EUR 9, BRL 11]

    }
}
```

#### Juntando as ordenações



Apenas como recordação, já que esse recurso não é da money-api e sim do Java 8, é possível também misturar mais de um ordenador, para isso basta utilizar o método **thenComparing**. Basicamente ele faz a ordenação e caso os valores tenham o mesmo peso, ao usar o compare retorne o valor zero, ele usará o outro ordenador, assim a ordem que for definida o sort influenciará no resultado da ordenação.


```java
public class SortMixMonetaryAmountNumberCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit euro = Monetary.getCurrency("EUR");
        CurrencyUnit real = Monetary.getCurrency("BRL");

        MonetaryAmount money = Money.of(10, euro);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, real);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> resultAsc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortNumber().thenComparing(MonetaryFunctions.sortCurrencyUnit()))
                .collect(Collectors.toList());//[USD 8, BRL 9, BRL 10, EUR 10, USD 10]
        List<MonetaryAmount> resultDesc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortNumberDesc().thenComparing(MonetaryFunctions.sortCurrencyUnitDesc()))
                .collect(Collectors.toList());//[USD 10, EUR 10, BRL 10, BRL 9, USD 8]
        //using currency first
        List<MonetaryAmount> resultCurrencyAsc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnit().thenComparing(MonetaryFunctions.sortNumber()))
                .collect(Collectors.toList());//[BRL 9, BRL 10, EUR 10, USD 8, USD 10]
        List<MonetaryAmount> resultCurrencyDesc = Stream.of(money, money2, money3, money4, money5)
                .sorted(MonetaryFunctions
                        .sortCurrencyUnitDesc().thenComparing(MonetaryFunctions.sortNumberDesc()))
                .collect(Collectors.toList());//[USD 10, USD 8, EUR 10, BRL 10, BRL 9]

    }
}
```

### Métodos de redução


A redução é o processo em que, dado uma lista de valores ele deverá retornar uma ou nenhuma saída, basicamente, a partir de uma lista transformá-lo para apenas um elemento, ou a ausência dele.

#### Somatório

A soma ou a redução pela soma é definido em: Dado uma lista de `MonetaryAmount` ele retornará um elemento com o somatório desses valores.


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

Salientando, caso tenha uma moeda diferente no somatório ele retornará uma exceção.

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

Caso seja necessário somar e converter para uma específica moeda, o moneta dar suporte para isso, basta informar uma implementação de `ExchangeRateProvider` e também a moeda que todos os valores serão convertidos.

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

#### Valor mínimo e valor máximo

É possível realizar a redução pelo valor máximo e mínimo, por exemplo, dado uma lista é possível retornar o maior ou o menor valor da lista.


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

Caso seja realizado a operação de mínimo e máximo com moedas diferentes, acontecerá uma exceção. É possível passar um `ExchangeRateProvider` para realizar a conversão e então a comparação.


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

### Predicates


Operação de predicate é quando dado uma entrada o retorno é um valor booleano, ou seja, *verdadeiro* ou *falso*. Dentro do Stream o predicate pode ser utilizado como filtragem, filtrar por uma moeda específica, ou para match, verificar se existe algum elemento da lista ou todos batem com a condição.


```java
public class BooleanMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Stream<MonetaryAmount> justDollars = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar));

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//true
    }
}
```

#### Predicate com moedas


Com o moneta é possível realizar predicate a partir da moeda, usando o inclusive e o exclusive. Ambos utilizam varargs, ou seja, é possível adicionar n elementos na condição:


* `isCurrency(CurrencyUnit... currencies)`: Retorna verdadeiro caso o `MonetaryAmount` tenha a uma das moedas especificadas.	
* `filterByExcludingCurrency(CurrencyUnit... currencies)`: Retorna verdadeiro caso o `MonetaryAmount` não tenha nenhuma das moedas especificadas.


```java
public class PredicateMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        CurrencyUnit euro = Monetary.getCurrency("EUR");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> justDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar)).collect(Collectors.toList());//[USD 10, USD 10, USD 8]

        boolean anyDollar = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allDollar = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//false

        List<MonetaryAmount> notDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.filterByExcludingCurrency(dollar)).collect(Collectors.toList());//[BRL 10, BRL 9]

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true

       
    }
}
```

#### Predicate com o valor numérico


Além de operações com moeda é possível realizar condições com o valor numérico do `MonetaryAmount`, os métodos têm o mesmo comportamento da interface do `MonetaryAmount`:


* `MonetaryFunctions.isLessThanOrEqualTo(monetaryAmount)`
* `MonetaryFunctions.isLessThan(monetaryAmount)`
* `MonetaryFunctions.isGreaterThan(monetaryAmount)`
* `MonetaryFunctions.isGreaterThanOrEqualTo(monetaryAmount)`
* `isBetween(MonetaryAmount min, MonetaryAmount max)`


```java
public class PredicateMonetaryAmountNumberValue {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> greaterThanZero = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isGreaterThan(Money.zero(dollar)))
                .collect(Collectors.toList());//[USD 10, USD 10, USD 10, USD 9, USD 8]

        boolean hasAnyGreaterThanZero = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isGreaterThan(Money.zero(dollar)));//true

        boolean allBetweenZeroAndTen = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isBetween(Money.zero(dollar),
                        Money.of(BigDecimal.TEN, dollar)));//true

    }
}
```

#### Interagindo com Predicates


Assim com o sort, também é possível realizar interação com o predicate, utilizando operações boolieanas, assim temos os métodos, **negate**, **and** e **or**.

```java
public class PredicateMonetaryAmountMix {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, dollar);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, dollar);
        MonetaryAmount money5 = Money.of(8, dollar);
        MonetaryAmount money6 = Money.of(8, dollar);

		List<MonetaryAmount> greaterThanZeroAndIsReal = Stream
				.of(money, money2, money3, money4, money5, money6)
				.filter(MonetaryFunctions.isGreaterThan(Money.zero(dollar))
						.and(MonetaryFunctions.isCurrency(real)))
				.collect(Collectors.toList());//[]
		List<MonetaryAmount> greaterThanZeroOrIsReal = Stream
				.of(money, money2, money3, money4, money5, money6)
				.filter(MonetaryFunctions.isGreaterThan(Money.zero(dollar))
						.or(MonetaryFunctions.isCurrency(real)))
				.collect(Collectors.toList());//[USD 10, USD 10, USD 10, USD 9, USD 8, USD 8]

		
		List<MonetaryAmount> notGreaterThan = Stream
				.of(money, money2, money3, money4, money5, money6)
				.distinct()
				.filter(MonetaryFunctions.isGreaterThan(Money.of(9, dollar))
						.negate())
				.collect(Collectors.toList());//[USD 9, USD 8]
    }
}
```

###  Agrupamento do MonetaryAmount

Outro recurso importante é o agrupamento das informações do dinheiro, com ele é possível agrupar, particionar e sumarizar o valor.

#### Agrupando pela moeda


O moneta traz uma função para facilitar o agrupamento dos `MonetaryAmount` a partir da Moeda.


```java
public class AggregateGroupByCurrencyMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Map<CurrencyUnit, List<MonetaryAmount>> groupByCurrency = Stream.of(money, money2, money3, money4, money5)
                .collect(MonetaryFunctions.groupByCurrencyUnit());//{USD=[USD 10, USD 10, USD 8], BRL=[BRL 10, BRL 9]}


    }
}
```

#### Particionamento


Com os predicates, explicados do subcapítulo anterior, é possível particionar as informações do dinheiro. O particionamento se baseia em dado uma condição os valores serão mapeados em true, os MonetaryAmounts que atenderam a condição, e false, os `MonetaryAmounts` que não atenderam a condição.


```java
public class AggregatePredicateMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");



        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Map<Boolean, List<MonetaryAmount>> mapDollar = Stream.of(money, money2, money3, money4, money5)
                .collect(Collectors.partitioningBy(
                        MonetaryFunctions.isCurrency(dollar)));//{false=[BRL 10, BRL 9], true=[USD 10, USD 10, USD 8]}

    }
}
```

#### Sumarizando a moeda



Outra forma de agrupar o dinheiro é a partir do agrupamento do `MonetaryAmount`, com ele é possível saber o valor mínimo, máximo, somatório, média e o número de elementos. Para isso é necessário informar a moeda, caso a moeda seja diferente da informada ele será ignorado.


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

O **Moneta** também tem suporte para agrupar os sumarizadores a partir da moeda.

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


Também é possível sumarizar todos os valores convertendo as moedas para a moeda definida no método, para isso basta informar um `ExchangeRateProvider`.


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
                .collect(ConversionOperators.summarizingMonetary(dollar, provider));

        MonetaryAmount min = summary.getMin();//USD 2.831248
        MonetaryAmount max = summary.getMax();//USD 25
        MonetaryAmount average = summary.getAverage();//USD 10.195416
        long count = summary.getCount();//5
        MonetaryAmount sum = summary.getSum();//50.97708
        
    }
}
```
