### Representando dinero y moneda con money-api



En money-api, será usado ese nombre en vez de JSR 354, también será necesario representar tanto el valor numérico en cuanto a la moneda. Para representar la moneda existe la interface ```CurrencyUnit``` esta necesita ser inmutable y thread-safe.

|Nombre del método| Descripción |Ejemplo|
| -- | -- | -- |
|```String getCurrencyCode()```|Retorna el código de la moneda, para las monedas que siguen el standard ISO serán retornado tres letras.|BRL para real brasileiro, USD para dólares americanos.
|```int getNumericCode()```|Retorna el código numérico de la moneda, asi como el código este posee tres dígitos.|986 para la moneda brasilera, 840 para dólares americanos.|
|``` int getDefaultFractionDigits()``` |Retorna el número de dígitos normalmente utilizado por la moneda.|BRL tiene dos y JPY no tiene.|


En este material la implementación utilizada será **moneta**, la implementación de referencia para esa especificación. Con este es posible crear una instancia de monedas de dos formas. La primera de ellas es utilizando el código de la moneda, si la moneda que siga el patrón de moneda será un ```String``` con tres letras.


```java
public class CurrencyExample1 {

    public static void main(String[] args) {

        CurrencyUnit currencyUnit = Monetary.getCurrency("BRL");
        String currencyCode = currencyUnit.getCurrencyCode();//BRL
        int numericCurrencyCode = currencyUnit.getNumericCode();//986
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2

    }
}
```

Otra forma simple es utilizando la clase ```Locale```, esa creación es muy interesante, por ejemplo, en una aplicación web de compras en que a partir de ```Locale``` de la solicitud será posible saber cual es la moneda del usuario que está accediendo a la aplicación.

```java

public class CurrencyExample2 {

    public static void main(String[] args) {
        CurrencyUnit currencyUnit = Monetary.getCurrency(Locale.US);
        String currencyCode = currencyUnit.getCurrencyCode();//USD
        int numericCurrencyCode = currencyUnit.getNumericCode();//840
        int fractionDigits = currencyUnit.getDefaultFractionDigits();//2
    }
}

```

Definido la representación de moneda el próximo paso será la representación del valor monetario, para eso, existe la interface ```MonetaryAmount```, uma característica importante es que todas las implementaciones necesitan ser inmutables y thread-safe. 

|Método| Descripción|
| -- | -- |
|```<R> R query(MonetaryQuery<R> query)```|Realiza query con valor monetário.|
|```MonetaryAmount with(MonetaryOperator operator)```|Realiza operaciones con el importe monetário.|
|```boolean isGreaterThan(MonetaryAmount amount)```|Retorna verdadero si la instancia es mayor que el valor pasado en el  parametro, asi sean iguales este retorna falso.|
|```boolean   isGreaterThanOrEqualTo(MonetaryAmount amount)```|Retorna verdadero si la instancia es mayor o igual que el valor pasado en el parametro, asi sean iguales este retorna verdadero.|
|```boolean isLessThan(MonetaryAmount amount)```|Retorna verdadero si una instancia es menor que el valor pasado en el  parametro, asi sean iguales este retorna falso.|
|```isLessThanOrEqualTo(MonetaryAmount amt)```|Retorna verdadero si una instancia es menor o igual que el valor pasado en el parametro, asi sean iguales este retorna verdadero.|
|```boolean isEqualTo(MonetaryAmount amount)```|Retorna verdadero si una instancia es igual al valor monetário pasado en el parametro.|
|```boolean isNegative()```|Retorna verdadero si es negativo|
|```boolean isNegativeOrZero()```|Retorna verdadero si es negativo o cero|
|```isPositive()```|Retorna verdadero si es positivo|
|```boolean isPositiveOrZero()```|Retorna verdadero si es positivo o igual a cero|
|```isZero()```|Verifica si el valor de dinero es cero.|
|```MonetaryAmount add(MonetaryAmount amount)```|Realiza la suma y retorna el resultado|
|```MonetaryAmount subtract(MonetaryAmount amount)```|Realiza la subtracción y retorna el resultado|
|```MonetaryAmount multiply(Number multiplicand)```|Realiza la multiplicación y retorna el resultado|
|```MonetaryAmount divide(Number divisor)```|Realiza a división|
|```MonetaryAmount remainder(Number divisor)```|Realiza la división y retorna el resto|
|```MonetaryAmount negate()```|Realiza la negación del importe monetario, osea, -este.
|```getCurrency()```|Retorna dinero de moneda.|

Dentro de moneta existen tres implementaciones para la interface ```MonetaryAmount```:


1. 
Money: la implementación standard, esta representa el valor numérico con BigDecimal.
1. 
RoundedMoney: asi como la implementación Money, representa el valor numérico con BigDecimal, la diferencia entre ellos es que con RoundedMoney es posible recibir un MonetaryOperator para ser llamada a cada operación, por ejemplo, a cada operación aritmética realizar una operación de redondeo.

1. 
FastMoney: la implementación que representa el valor número con primitivo long, de las implementaciones presentadas esta es la mas rápida, cerca de quince veces mas rápidas que las otras dos, además de ser mas ligero en la creación. Sin embargo esta posee una mayor limitación en relación a la precisión, si es necesario trabajar con esa precisión, las operaciones no puede exceder de 5 puntos decimales.
