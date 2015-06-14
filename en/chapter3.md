## Manipulate and extract information from MonetaryAmount


Also beyond do basic operations such arithmetic operations, some time is important can extract and convert some money informations, for example, extract just the little part of the money, the cents, or just the value on the right of comma. To do operations like those, there are the `MonetaryOperator` and `MonetaryQuery` interfaces. Both has just one method to be implemented, so they are functional interface.


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

