## Manipulando y seleccionando información de MonetaryAmount


Además de realizar operaciones basicas como las operaciones aritméticas, algunas veces es necesario seleccionar y convertir informaciones de dinero, por ejemplo, seleccionar la menor parte de centavos, obtener solo los centavos, o solo la mayor parte de dinero. Con ese objetivo existe las interfaces ```MonetaryOperator``` y ```MonetaryQuery```. Ambas interfaces poseen solo un método para ser implementados, osea, son interfaces funcionales.


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

