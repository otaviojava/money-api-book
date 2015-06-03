### Métodos de criação:


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
 




