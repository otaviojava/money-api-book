### MonetaryQuery vs MonetaryOperator 


Pero con ```MonetaryQuery``` es posible también retornar ```MonetaryAmount``` y asi tenemos el mismo resultado que  ```MonetaryOperator```, y cual es el punto de tener dos interfaces? El punto de tener las dos interfaces es por cuestión de nomenclatura y estandarización. ```MonetaryQuery``` tiene el objetivo seleccionar y buscar informaciones dentro de  ```MonetaryAmount```, ya ```MonetaryOperator``` tiene el objetivo de realizar operaciones con dinero.

```java
public class DifferenceMonetaryQueryMonetaryOperator {

    public  static void main(String[] args) {

        MonetaryQuery<MonetaryAmount> doubleQuery = m -> m.multiply(2);
        MonetaryOperator doubleOperator = m -> m.multiply(2);

    }
}
```
