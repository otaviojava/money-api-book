### MonetaryQuery vs MonetaryOperator 

Operations done through `MonetaryQuery` and `MonetaryOperator` might be equivalent. The reason why we have them is separate interfaces is that we need to keep the concerns separate.  `MonetaryQuery` is responsible for retrieving information about the  `MonetaryAmount` where `MonetaryOperator` is responsible for applying operations on `MonetaryAmount`. 

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {
        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
