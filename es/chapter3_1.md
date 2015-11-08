### MonetaryOperator


```MonetaryOperator``` es una interface funcional que recibe un ```MonetaryAmount``` y retorna un ```MonetaryAmount```. Esa interface es muy discutida al hablarse sobre la implementación de ```RoundedMoney``` que la utiliza como agente redondeador. Con esta interface es posible realizar operaciones de redondeo, retornar parte de dinero, el doble del valor, etc. 

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

Para ejecutarlo basta llamar el método apply de la interface o llamar el método **with** dentro de ```MonetaryAmount```.


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


Moneta trae por estándar algunas implementaciones de `MonetaryOperator`, la clase `MonetaryOperators`. esta es una clase utilitaria que trae algunas funcionalidades importantes y algunas veces muy cotidiano dentro de la vida de un desarrollador Java, como:



* **reciprocal()** Retorna dinero como reciprocal, multiplicado por el valor inverso (1/n).
* **permil(Number number)** retorna el valor permil de dinero, por ejemplo, **permil(10)** de `EUR 2.35` retornará EUR `0.0235`.
* **percent(Number number)** retorna el percentual de dinero, por ejemplo, **percent(10)** de `EUR 200.00` retornará `EUR 20.00`.
* **minorPart()** retorna el valor que se encuentra a la derecha del punto decimal, por exemplo, la menor parte de `EUR 2.35` es ```EUR 0.35```.
* **majorPart()** retorna el valor entero de dinero, por ejemplo, la menor parte de `EUR 2.35` es `EUR 2`.
* **rounding()** realiza el proceso de redondeo de dinero, para saber el número de decimales despues del punto es recuperado la información del método getDefaultFractionDigits() de la interface CurrencyUnit.
* **exchange(CurrencyUnit currency)** dado un Dinero ese operador realizará el intercambio de la moneda, osea, este solo va intercambiar la moneda no llevando en consideración su cotización, por ejemplo, `EUR 2.35` **exchange('BRL')** retornará `BRL 2.35`. This method is in the class `ConversionOperators`

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
