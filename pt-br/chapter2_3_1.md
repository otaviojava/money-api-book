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