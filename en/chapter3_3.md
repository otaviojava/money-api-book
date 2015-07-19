### MonetaryQuery vs MonetaryOperator 

Operations done using `MonetaryQuery` and `MonetaryOperator` might be equivalent. The reason why we have them as separate interfaces is to keep their fuctions separate. `MonetaryQuery` is responsible for retrieving information about the `MonetaryAmount` whereas `MonetaryOperator` is responsible for applying operations on the `MonetaryAmount`. 

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {
        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
