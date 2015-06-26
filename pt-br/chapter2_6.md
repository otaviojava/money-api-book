### NumberValue: A representação da parte numérica do money


Em alguns momentos é importante recuperar e tratar as informações do dinheiro de forma separada, para isso, a interface dispõe de método para recuperar tanto a moeda quanto o valor monetário. Para a moeda ele obviamente retorna a interface ```CurrencyUnit``` e para representar o valor numérico é retornado a classe ```NumberValue```. Essa classe é filha da ```Number``` do Java, assim é possível recuperar para os tipos primitivos básicos do Java (```int```, ```long```, ```float```, ```double``` etc.).


```java
public class RetrieveInformationMethods {

    public  static  void main(String[] args) {

        MonetaryAmount money = Money.of(10, Monetary.getCurrency("BRL"));
        CurrencyUnit currency = money.getCurrency();
        Number number = money.getNumber();

    }
}
```

Além disso, com o ```NumberValue``` é possível realizar mais algumas operações, por exemplo, é possível definir qual classe será recuperada a partir do ```NumberValue```, desde que essa classe seja filha da classe ```Number```. No caso da implementação de referência serão todas as classes que são ```Number``` e estão dentro do JDK.


```java
public class RetrieveInformationMethods2 {

    public  static  void main(String[] args) {


        MonetaryAmount money = Money.of(BigDecimal.valueOf(10.21), Monetary.getCurrency("BRL"));
        NumberValue number = money.getNumber();
        int precision = number.getPrecision();//4
        int scale = number.getScale();//2
        long amountFractionDenominator = number.getAmountFractionDenominator();//21
        long amountFractionNumerator = number.getAmountFractionNumerator();//10
        Class<?> numberType = number.getNumberType();//java.math.BigDecimal
        BigDecimal value = number.numberValue(BigDecimal.class);
        Integer integerValue = number.numberValue(Integer.class);


    }
}
```
