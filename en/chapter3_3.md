### MonetaryQuery vs MonetaryOperator 

Considering with `MonetaryQuery` may return a `MonetaryAmount`, so it's possible to have the same result of a `MonetaryOperator`, but what is the goal of have two interfaces? The goal is just to nomenclature and standarization. `MonetaryQuery` has the goal of retrieve information within a `MonetaryAmount`, with `MonetaryOperator` has the goal to do operations with `MonetaryAmount`.

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {

        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
