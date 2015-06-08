### MonetaryQuery vs MonetaryOperator 


Mas com o ```MonetaryQuery``` é possível também retornar ```MonetaryAmount``` e assim temos o mesmo resultado que um ```MonetaryOperator```, assim qual é o objetivo de ter duas interfaces? O Objetivo de terem as duas interfaces é por questão de nomenclatura e padronização. ```MonetaryQuery``` tem o objetivo extrair e buscar informações dentro do ```MonetaryAmount```, já o ```MonetaryOperator``` tem o objetivo de realizar operações com o dinheiro.

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {

        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
