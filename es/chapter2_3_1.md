#### Métodos de creación para RoundedMoney



Además de los métodos comunes de construcción la clase ```RoundedMoney```, posee otras formas de parámetros para que sea posible informar a ```MonetaryOperator``` para ser ejecutado después de cada operación de ```MonetaryAmount```, vale recordar, la principal característica de esa clase es realizar ese tipo de operación, asi no sea necesario, otra implementación es recomendada.


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
