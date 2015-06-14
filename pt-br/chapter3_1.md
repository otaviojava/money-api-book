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


O moneta traz por padrão algumas implementações de ```MonetaryOperator```, a classe ```MonetaryOperators```. Ela é uma classe utilitária que traz algumas funcionalidades importantes e algumas vezes muito corriqueira dentro da vida de um desenvolvedor Java, como:



* **reciprocal()** Retorna o dinheiro como reciprocal, multiplicando pelo valor inverso (1/n).
* **permil(Number number)** retorna o valor permil do dinheiro, por exemplo, **permil(10)** de `EUR 2.35` retornará EUR `0.0235`.
* **percent(Number number)** retorna o percentual de um dinheiro, por exemplo, **percent(10)** de `EUR 200.00` retornará `EUR 20.00`.
* **minorPart()** retorna o valor que se encontra na direita da vírgula, por exemplo, a menor parte de `EUR 2.35` é ```EUR 0.35```.
* **majorPart()** retorna o valor inteiro do dinheiro, por exemplo, a maior parte de `EUR 2.35` é `EUR 2`.
* **rounding()** realiza o processo de arredondamento do dinheiro, para saber o número de casas após a vírgula é recuperado a informação do método getDefaultFractionDigits() da interface CurrencyUnit.
* **exchange(CurrencyUnit currency)** dado um dinheiro esse operador realizará a troca da moeda, ou seja, ele apenas vai trocar mudar a moeda não levando em consideração a sua cotação, por exemplo, `EUR 2.35` **exchange('BRL')** retornará `BRL 2.35`.

```java

public class MonetaryOperatorsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        CurrencyUnit dollar = Monetary.getCurrency(Locale.US);
        
        MonetaryAmount money = Money.of(120.231, currency);
        
        MonetaryAmount majorParteResult = money.with(MonetaryOperators.majorPart());//BRL 120
        MonetaryAmount minorPartResult = money.with(MonetaryOperators.minorPart());//BRL 0.231
        MonetaryAmount percentResult = money.with(MonetaryOperators.percent(20));//BRL 24.0462
        MonetaryAmount permilResult = money.with(MonetaryOperators.permil(100));//BRL 12.0231
        MonetaryAmount roundingResult = money.with(MonetaryOperators.rounding());//BRL 120.23
        MonetaryAmount resultExchange = money.with(MonetaryOperators.exchange(dollar));//USD 120.231
    }
}
```