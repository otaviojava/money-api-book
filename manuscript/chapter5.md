## Exchange Rate

Internationalization is the process of increasing the involvement of enterprises in international markets. This can have an impact on multiple levels in culture, economy or politics. It affects daily life in that it's easy to find out what is happening across the world, to discover cultures just by surfing the Internet or to simply buy products or services from other countries. Software needs to follow this trend, it needs to allow users to easily interact with each other across the globe. It is exactly the same case with money; software has an obligation to support different currencies.


Talking about money and internationalization will lead you to think about the interaction between different currencies. What would happen if we tried to do the following operation?

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

Using **Moneta**, you will get an exception with the error informing you that you are trying to sum money with distinct currencies. It is a “correct error” even if the dollar has the exact same value as one Brazilian real. In order for this summing operation to succeed, it first needs to use the exchange rate between the two currencies. The exchange rate is the rate at which one currency will be exchanged for another.
