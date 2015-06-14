## Exchange Rate


The internationalization is the approach applied on several areas, basically, is a change of culture, economy and politics on considerable places on the world. With internalization may to know what is happening in across the world instantaneously, know cultures just surfing in the Internet as well as buy products or services in other countries. The software needs to follow this trend, in other words, allow interaction with users in all globe. With money it isn't different, the software has the obligation to be ready to it.

Talking about money and internationalization is trivial think the interaction between currencies, but what happen when try do operation with different currencies?


```java
public class MistakeExample {


    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);
        MonetaryAmount result = money.add(money2);//javax.money.MonetaryException: Currency mismatch: USD/BRL
    }
}
```


Utilizando o money-api, ele gerará uma exceção afirmando que existe um erro ao tentar somar dinheiro com moedas diferentes, nesse caso real com dólar. Essa exceção foi lançada de forma correta uma vez que não necessariamente um dólar equivale a um real. Para realiza tal operação de somatório entre moedas diferentes é necessário primeiro realizar a cotação da moeda. A taxa de câmbio é a relação das moedas entre dois países que resulta no preço de uma delas com relação a outra. 