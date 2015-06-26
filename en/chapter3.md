## Querying/Operation on MonetaryAmount

`MonetaryOperator` and `MonetaryQuery` are functional interfaces, they provide abstraction for querying and operating on monetary values like conversions and retrieving the fractional parts (cents).    

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
