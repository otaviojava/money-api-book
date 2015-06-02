# Mas ninguém passou por isso antes? Conhecimento da API


No capítulo anterior foi discutido as vantagens de se trazer o dinheiro como um tipo em vez de tratá-lo como apenas como tipo primitivo. Mas será que ninguém nunca passou por esse problema antes? A arte de “reinventar a roda” além de ser muito custosa, já que demanda muito tempo, passa a ser muito perigosa já que se tende a passar pelos mesmos problemas que um programador mais experiente já enfrentou. Uma famosa frase de *Clarice Lispecto*r diz: *"Quem caminha sozinho pode até chegar mais rápido, mas aquele que vai acompanhado, com certeza vai mais longe."*, ou seja, a melhor estratégia é se unir à um framework que já faz isso, assim é possível trocar experiências com programadores que já passaram por isso, não repetir os mesmos erros, contribuir com essa ferramenta, dessa forma, ela ficará cada mais e mais robusta e com menos erros.

Assim se pode unir a uma solução favorita para desenvolver o seu tipo dinheiro. Como lidar com esse tipo de problema é cada vez mais comum no mundo do desenvolvimento de software, o que acontece se cada um utilizar a sua solução específica? Por muitas razões se pode optar por uma solução, por exemplo, uma boa documentação, licença, razões comerciais, etc. Assim como integrar com cada uma delas? Caso se escolha uma é possível trocar para uma outra? Sabemos que ficar preso em um vendor não é algo bons para os negócios. Com esse objetivo nasce uma especificação Java para lidar com o tipo dinheiro a JSR 354. Ela tem como principal objetivo tratar dinheiro. 

Uma vez definindo a interface comum, cada empresa ou desenvolvedor poderá optar pela sua solução favorita ou trocá-la de forma transparente, para isso, basta que essa API implemente essa especificação. Esse comportamento é bem semelhante numa troca de banco de dados relacional no mundo Java, caso a aplicação siga a especificação e use as interfaces do JDBC, se pode trocar de banco tranquilamente, para isso, é necessário apenas trocar o driver, implementação, e tudo continuará funcionando desde que sua aplicação use apenas as interfaces.

Relembrando o conceito de dinheiro, de forma resumida, o dinheiro é composto por duas partes, a parte do valor que é a quantidade, assim representada de forma numérica, mas apenas com esse valor não conseguimos fazer muita coisa, precisamos da moeda. A moeda representa o “sistema do dinheiro” em comum uso, especialmente dentro de uma nação, seguindo essa definição o real, peso, dólar e euros são tipos de moedas.


Na money-api, será usado esse nome em vez de JSR 354, também será necessário representar tanto o valor numérico quanto a moeda. Para representar a moeda existe a interface ```CurrencyUnit``` ela precisa ser imutável e thread-safe.

|Nome do método| Descrição |Exemplo|
| -- | -- | -- |
|```String getCurrencyCode()```|Retorna o código da moeda, para as moedas que seguem o ISO isso serão retornados três letras.|BRL para o real brasileiro, USD para dólares americanos.
|```int getNumericCode()```|Retorna o código numérico da moeda, assim como o código ele possui três dígitos.|986 para a moeda brasileira, 840 para dólares americanos.|
|``` int getDefaultFractionDigits()``` |Retorna o número de dígitos normalmente utilizado pela moeda.|BRL tem dois e JPY não tem.|


Nesse material a implementação utilizada será o **moneta**, a implementação de referência para essa especificação. Com ele é possível criar uma instância de moedas de duas formas. A primeira delas é utilizando o código da moeda, caso seja uma moeda que siga o padrão de moeda será uma ```String``` com três letras.


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

Definido a representação da moeda o próximo passo será a representação do valor monetário, para isso, existe a interface MonetaryAmount, uma característica importante é que todas as implementações precisam ser imutável e thread-safe. 

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

