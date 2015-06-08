## Cotação


A internacionalização é um conceito aplicado em diversas áreas, basicamente, é uma troca de cultura, economia e política em diversos lugares do mundo. Com a internacionalização é possível, por exemplo, saber o que acontece do outro lado mundo de maneira instantânea, conhecer lugares e culturas sem sair de casa além de adquirir produtos. É muito comum exportar e importar diversas coisas, produtos, serviços, assinaturas, etc. Seguindo a tendência mundial os softwares também precisam estar preparados para uma arquitetura internacionalizada, ou seja, permitir a interação de usuário em diversos pontos do mundo. Com o dinheiro não é diferente, é necessário estar pronto para isso.

Falando de internacionalização e dinheiro é natural que precise interagir com diversas moedas, mas o que acontece ao realizar operações com moedas diferentes?


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