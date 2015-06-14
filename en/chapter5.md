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


Using the **Moneta**, will happen an exception with the error to try sum money with currencies distinct. It is a “right error” once the dollar, not necessarily, has the value of one Brazilian real. To do this operation of sum among currencies, it needs first do the exchange rate. The exchange rate is the rate at which one currency will be exchanged for another.