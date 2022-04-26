## Mas ninguém passou por isso antes? Conhecimento da API


No capítulo anterior foi discutido as vantagens de se trazer o dinheiro como um tipo em vez de tratá-lo como apenas como tipo primitivo. Mas será que ninguém nunca passou por esse problema antes? A arte de “reinventar a roda” além de ser muito custosa, já que demanda muito tempo, passa a ser muito perigosa já que se tende a passar pelos mesmos problemas que um programador mais experiente já enfrentou. Uma famosa frase de *Clarice Lispecto*r diz: *"Quem caminha sozinho pode até chegar mais rápido, mas aquele que vai acompanhado, com certeza vai mais longe."*, ou seja, a melhor estratégia é se unir à um framework que já faz isso, assim é possível trocar experiências com programadores que já passaram por isso, não repetir os mesmos erros, contribuir com essa ferramenta, dessa forma, ela ficará cada mais e mais robusta e com menos erros.

Assim se pode unir a uma solução favorita para desenvolver o seu tipo dinheiro. Como lidar com esse tipo de problema é cada vez mais comum no mundo do desenvolvimento de software, o que acontece se cada um utilizar a sua solução específica? Por muitas razões se pode optar por uma solução, por exemplo, uma boa documentação, licença, razões comerciais, etc. Assim como integrar com cada uma delas? Caso se escolha uma é possível trocar para uma outra? Sabemos que ficar preso em um vendor não é algo bons para os negócios. Com esse objetivo nasce uma especificação Java para lidar com o tipo dinheiro a JSR 354. Ela tem como principal objetivo tratar dinheiro. 

Uma vez definindo a interface comum, cada empresa ou desenvolvedor poderá optar pela sua solução favorita ou trocá-la de forma transparente, para isso, basta que essa API implemente essa especificação. Esse comportamento é bem semelhante numa troca de banco de dados relacional no mundo Java, caso a aplicação siga a especificação e use as interfaces do JDBC, se pode trocar de banco tranquilamente, para isso, é necessário apenas trocar o driver, implementação, e tudo continuará funcionando desde que sua aplicação use apenas as interfaces.

Relembrando o conceito de dinheiro, de forma resumida, o dinheiro é composto por duas partes, a parte do valor que é a quantidade, assim representada de forma numérica, mas apenas com esse valor não conseguimos fazer muita coisa, precisamos da moeda. A moeda representa o “sistema do dinheiro” em comum uso, especialmente dentro de uma nação, seguindo essa definição o real, peso, dólar e euros são tipos de moedas.

### Acesso ao código fonte e instalação

Como toda especificação no Java, JSR, além da API ela possui uma implementação de referência, como o próprio nome diz, essa implementação servirá de base para as próximas implementações. No caso da JSR 354, money-api, a implementação de referência é o Moneta (Para mais informações em relação ao código-fonte do projeto acesse [https://github.com/JavaMoney/jsr354-ri](https://github.com/JavaMoney/jsr354-ri)). Basicamente, é possível utilizar o moneta de duas formas:

A primeira delas é acessando o seu repositório de dependência do maven  ([http://mvnrepository.com/artifact/org.javamoney/moneta](http://mvnrepository.com/artifact/org.javamoney/moneta)), assim caso o seu projeto use o maven, por exemplo, basta adicionar a dependência do moneta em seu projeto da seguinte forma:

```
        <dependency>
            <groupId>org.javamoney</groupId>
            <artifactId>moneta</artifactId>
            <version>moneta_version</version>
        </dependency>
```

Caso seja Gradle:
```
'org.javamoney:moneta:moneta_version'
```

Caso seja Yvi:
```
<dependency org="org.javamoney" name="moneta" rev="moneta_version"/>
```


A outra forma é baixando o código-fonte e o compilando, para isso será necessário seguir alguns passos, mas basicamente é baixar o parent o instalá-lo e em seguida instalar o Moneta. Vale lembrar que para realizar esse procedimento é necessário ter o git, Java 8 e o maven instalado e configurado em sua máquina.


Baixar o código do javamoney-parent, para isso basta executar o seguinte comando:

```
git clone https://github.com/JavaMoney/javamoney-parent.git
```

Em seguida acesse a pasta:
```
cd javamoney-parent
```

Compile e código-fonte e o instale em seu repositório local, no nosso caso também pularemos os testes apenas para que seja mais rápido:

```
mvn clean install -Dmaven.test.skip
```

Pronto, uma vez feito isso o próximo passo será realizar a instalação do Moneta.

Baixar o código do Moneta, para isso basta executar o seguinte comando:

```
git clone git@github.com:JavaMoney/jsr354-ri.git
```

Em seguida acesse a pasta:

```
cd jsr354-ri
```

Compile e código-fonte e o instale em seu repositório local, no nosso caso também pularemos os testes apenas para que seja mais rápido:

```
mvn clean install -Dmaven.test.skip
```


Pronto, código-fonte instalado e poderá ser utilizado tranquilamente. No caso do livro estamos usando como base a master do repositório, ou seja, é recomendável que baixe o código-fonte e o instale localmente. Para ter acesso ao código-fonte de exemplo basta acessar:

* [https://github.com/otaviojava/money-api-book-samples](https://github.com/otaviojava/money-api-book-samples)


### Representando dinheiro e moeda com o money-api



Na money-api, será usado esse nome em vez de JSR 354, também será necessário representar tanto o valor numérico quanto a moeda. Para representar a moeda existe a interface ```CurrencyUnit``` ela precisa ser imutável e thread-safe.

|Nome do método| Descrição |Exemplo|
| -- | -- | -- |
|```String getCurrencyCode()```|Retorna o código da moeda, para as moedas que seguem o ISO isso serão retornados três letras.|BRL para o real brasileiro, USD para dólares americanos.
|```int getNumericCode()```|Retorna o código numérico da moeda, assim como o código ele possui três dígitos.|986 para a moeda brasileira, 840 para dólares americanos.|
|``` int getDefaultFractionDigits()``` |Retorna o número de dígitos normalmente utilizado pela moeda.|BRL tem dois e JPY não tem.|


Nesse material a implementação utilizada será o **Moneta**, a implementação de referência para essa especificação. Com ele é possível criar uma instância de moedas de duas formas. A primeira delas é utilizando o código da moeda, caso seja uma moeda que siga o padrão de moeda será uma ```String``` com três letras.


```java
public class CurrencyExample1 {

    public static void main(String[] args) {

        CurrencyUnit currencyUnit = Monetary.getCurrency("BRL");
        String currencyCode = currencyUnit.getCurrencyCode();//BRL
        int numericCurrencyCode = currencyUnit.getNumericCode();//986
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2

    }
}
```

A outra forma simples é utilizando a classe ```Locale```, essa criação é muito interessante, por exemplo, em uma aplicação web de compras em que a partir do ```Locale``` do request será possível saber qual é a moeda do usuário que está acessando a aplicação.

```java

public class CurrencyExample2 {

    public static void main(String[] args) {
        CurrencyUnit currencyUnit = Monetary.getCurrency(Locale.US);
        String currencyCode = currencyUnit.getCurrencyCode();//USD
        int numericCurrencyCode = currencyUnit.getNumericCode();//840
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2
    }
}

```

Definido a representação da moeda o próximo passo será a representação do valor monetário, para isso, existe a interface ```MonetaryAmount```, uma característica importante é que todas as implementações precisam ser imutável e thread-safe. 

|Método| Descrição|
| -- | -- |
|```<R> R query(MonetaryQuery<R> query)```|Realiza query com o valor monetário.|
|```MonetaryAmount with(MonetaryOperator operator)```|Realiza operações com o montante monetário.|
|```boolean isGreaterThan(MonetaryAmount amount)```|Retorna verdadeiro se a instância é maior que o valor passado no parâmetro, caso sejam iguais ele retornará falso.|
|```boolean   isGreaterThanOrEqualTo(MonetaryAmount amount)```|Retorna verdadeiro se a instância é maior ou igual que o valor passado no parâmetro, caso sejam iguais ele retornará verdadeiro.|
|```boolean isLessThan(MonetaryAmount amount)```|Retorna verdadeiro se a instância é menor que o valor passado no parâmetro, caso sejam iguais ele retornará falso.|
|```isLessThanOrEqualTo(MonetaryAmount amt)```|Retorna verdadeiro se a instância é menor ou igual que o valor passado no parâmetro, caso sejam iguais ele retornará verdadeiro.|
|```boolean isEqualTo(MonetaryAmount amount)```|Retorna verdadeiro caso a instância é igual ao valor monetário passado no parâmetro.|
|```boolean isNegative()```|Retorna verdadeiro se negativo|
|```boolean isNegativeOrZero()```|Retorna verdadeiro se é negativo ou zero|
|```isPositive()```|Retorna verdadeiro se é positivo|
|```boolean isPositiveOrZero()```|Retorna verdadeiro se positivo ou igual a zero|
|```isZero()```|Verifica se o valor do dinheiro é zero.|
|```MonetaryAmount add(MonetaryAmount amount)```|Realiza a soma e retorna o resultado|
|```MonetaryAmount subtract(MonetaryAmount amount)```|Realiza a subtração e retorna o reusltado|
|```MonetaryAmount multiply(Number multiplicand)```|Realiza a multiplicação e retorna o resultado|
|```MonetaryAmount divide(Number divisor)```|Realiza a divisão|
|```MonetaryAmount remainder(Number divisor)```|Realiza a divisão e retorna o resto|
|```MonetaryAmount negate()```|Realiza a negação do montante monetário, ou seja, -this.
|```getCurrency()```|Retorna o dinheiro da moeda.|

Dentro do **Moneta** existem três implementações para a interface ```MonetaryAmount```:


1. 
Money: a implementação padrão, ela representa o valor numérico com o BigDecimal.
1. 
RoundedMoney: assim como a implementação Money, representa o valor numérico com o BigDecimal, a diferença entre eles é que com o RoundedMoney é possível receber um MonetaryOperator para ser chamada a cada operação, por exemplo, a cada operação aritmética realizar uma operação de arredondamento.

1. 
FastMoney: a implementação que representa o valor número com o primitivo long, das implementações apresentadas ela é a mais rápida, cerca de quinze vezes mais rápidas que as outras duas, além de ser mais leve na criação. Porém ela possui uma maior limitação em relação a precisão, caso seja necessário trabalhar com essa precisão, as operações não podem ultrapassar de cinco casas decimais.

### Métodos de criação


Para criar uma instância de ```MonetaryAmount```, todas as implementações seguem o mesmo padrão de nomenclatura utilizando, com uma pequena exceção no ```RoundedMoney``` uma vez que ele pode receber um ```MonetaryOperator``` para trabalhar como “arredondador” em cada operação. Listando os mais importantes temos o total de três métodos, que são:

* O método **of** passando um number e um código de moeda.
* O método **zero** passando um CurrencyUnit.
* O método **ofMinor** passando um long e uma moeda, esse long será tratado como centavos e será convertido levando em consideração a moeda, por exemplo, 200 cents equivalem a dois dólares.


```java
public class MethodsCreationMoney {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(BigDecimal.TEN, currency); //BRL 10
        MonetaryAmount zero = Money.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = Money.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = Money.ofMinor(currency, 100_00);//BRL 10
    }
}
```


```java
public class MethodsCreationsFastMoney {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(BigDecimal.TEN, currency); //BRL 10
        MonetaryAmount zero = FastMoney.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = FastMoney.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = FastMoney.ofMinor(currency, 100_00);//BRL 10
    }
}
```


```java
public class MethodsCreationRoundedMoney {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = RoundedMoney.of(BigDecimal.TEN, currency); //BRL 10
        MonetaryAmount zero = RoundedMoney.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = RoundedMoney.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = RoundedMoney.ofMinor(currency, 100_00);//BRL 10
    }
}
```
 

#### Métodos de Criação para o RoundedMoney



Além dos métodos comuns de construção a classe ```RoundedMoney```, possui outras formas de parâmetros para que seja possível informar o ```MonetaryOperator``` para ser executado após cada operação do ```MonetaryAmount```, vale lembrar, a principal característica dessa classe é realizar esse tipo de operação, caso não seja necessário, outra implementação é recomendada.


```java
public class RoundedMoneyCreation2 {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = RoundedMoney.of(BigDecimal.TEN, currency, MonetaryOperators.rounding()); //BRL 10
        MonetaryAmount zero = RoundedMoney.zero(currency);//BRL 0
        MonetaryAmount moneyFromCurrencyCode = RoundedMoney.of(10, "USD");//USD 10
        MonetaryAmount moneyFromCents = RoundedMoney.ofMinor(currency, 100_00);//BRL 10
    }
}
```

### Operações Aritméticas


Com o ```MonetaryAmount``` é possível realizar operações como subtração e soma, salientando que as implementações são imutáveis, ou seja, o resultado resultará em uma nova instância. Ao realizar operações que recebem um ```MonetaryAmount``` o resultado será também um ```MonetaryAmount```, mas da implementação da instância que chamou o método.

```java
public class ArithmeticOperations {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(BigDecimal.TEN, currency);
        MonetaryAmount money2 = FastMoney.of(BigDecimal.TEN, currency);
        MonetaryAmount addResult = money.add(money2);//BRL 20 Money implementation
        MonetaryAmount subtractResult = money2.subtract(addResult);//BRL -10 FastMoney implementation
    }
}
```

Para as operações que de multiplicação, divisão e resto é necessário passar um parâmetro do tipo ```Number```.

```java
public class ArithmeticOperations2 {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(100, currency);
        Number number = 20;
        MonetaryAmount divideResult = money.divide(number);//BRL 5
        MonetaryAmount remainderResult = money.remainder(30);//BRL 10
        MonetaryAmount resultMultiply = money.multiply(5);//BRL 500
    }
}
```

Também é possível realizar operações de sinais com o ```MonetaryAmount```.



```java
public class ArithmeticOperations3 {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(100, currency);
        MonetaryAmount negateResult = money.negate();//BRL -100
        MonetaryAmount plusResult = money.plus();//BRL 100
    }
}
```

### Operações boolianas

Com o MonetaryAmount é possível realizar comparações em relações a outros ```MonetaryAmount```, com ele é possível, por exemplo, verificar se um dinheiro é maior, menor ou igual ao outro.


```java
    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(10, currency);
        MonetaryAmount money2 = Money.of(20, currency);
        boolean equalsToResult = money.isEqualTo(money2);//false
        boolean greaterThan = money.isGreaterThan(money2);//false
        boolean greaterThanOrEqualTo = money.isGreaterThanOrEqualTo(money2);//false
        boolean lessThan = money.isLessThan(money2);//true
        boolean lessThanOrEqualTo = money.isLessThanOrEqualTo(money2);//true
        boolean negative = money.isNegative();//false
        boolean negativeOrZero = money.isNegativeOrZero();//false
        boolean positive = money.isPositive();//true
        boolean positiveOrZero = money.isPositiveOrZero();//true
        boolean zero = money.isZero();//false
    }
}
```


### NumberValue: A representação da parte numérica do money


Em alguns momentos é importante recuperar e tratar as informações do dinheiro de forma separada, para isso, a interface dispõe de método para recuperar tanto a moeda quanto o valor monetário. Para a moeda ele obviamente retorna a interface ```CurrencyUnit``` e para representar o valor numérico é retornado a classe ```NumberValue```. Essa classe é filha da ```Number``` do Java, assim é possível recuperar para os tipos primitivos básicos do Java (```int```, ```long```, ```float```, ```double``` etc.).


```java
public class RetrieveInformationMethods {

    public  static  void main(String[] args) {

        MonetaryAmount money = Money.of(10, Monetary.getCurrency("BRL"));
        CurrencyUnit currency = money.getCurrency();
        Number number = money.getNumber();

    }
}
```

Além disso, com o ```NumberValue``` é possível realizar mais algumas operações, por exemplo, é possível definir qual classe será recuperada a partir do ```NumberValue```, desde que essa classe seja filha da classe ```Number```. No caso da implementação de referência serão todas as classes que são ```Number``` e estão dentro do JDK.


```java
public class RetrieveInformationMethods2 {

    public  static  void main(String[] args) {


        MonetaryAmount money = Money.of(BigDecimal.valueOf(10.21), Monetary.getCurrency("BRL"));
        NumberValue number = money.getNumber();
        int precision = number.getPrecision();//4
        int scale = number.getScale();//2
        long amountFractionDenominator = number.getAmountFractionDenominator();//21
        long amountFractionNumerator = number.getAmountFractionNumerator();//10
        Class<?> numberType = number.getNumberType();//java.math.BigDecimal
        BigDecimal value = number.numberValue(BigDecimal.class);
        Integer integerValue = number.numberValue(Integer.class);


    }
}
```



