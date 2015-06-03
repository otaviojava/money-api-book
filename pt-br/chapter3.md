## Manipulando e extraindo informação do MonetaryAmount


Além de realizar operações básicas como as operações aritméticas, algumas vezes é necessário extrair e converter informações do dinheiro, por exemplo, extrair a menor parte do centavos, obter apenas os centavos, ou apenas a maior parte do dinheiro. Com esse intuito existe as interfaces ```MonetaryOperator``` e ```MonetaryQuery```. Ambas as interfaces possuem apenas um método para ser implementados, ou seja, são interfaces funcionais.


```java

@FunctionalInterface
public interface MonetaryOperator{

    MonetaryAmount apply(MonetaryAmount amount);
}


@FunctionalInterface
public interface MonetaryQuery<R>{

    R queryFrom(MonetaryAmount amount);
}

```

