## Cotización


La internacionalización es un concepto aplicado en diversas áreas, basicamente, es un intercambio de cultura, economía y política en diversos lugares del mundo. Con la internacionalización es posible, por ejemplo, saber lo que sucede del otro lado del mundo de manera instantanea, conocer lugares y culturas sin salir de casa ademas de adquirir productos. Es muy común exportar e importar diversas cosas, productos, servicios, asignaturas, etc. Siguiendo la tendencia mundial los software también necesitan estar preparados para una arquitetura internacionalizada, osea, permitir la interacción del usuario en diversos lugares del mundo. Com dinero no es diferente, es necesario estar listo para esto.

Hablando de internacionalización y dinero es natural que necesite interactuar con diversas monedas, pero que pasa al realizar operaciones con monedas diferentes?


```java
public class MistakeExample {


    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        MonetaryAmount money = FastMoney.of(10, dollar);
        MonetaryAmount money2 = FastMoney.of(10, real);
        MonetaryAmount result = money.add(money2);//javax.money.MonetaryException: Currency mismatch: USD/BRL
    }
}
```


Utilizando money-api, este generará una excepción afirmando que existe um error al probar sumar dinero con monedas diferentes, en este caso real con dólar. Esta excepción fue lanzada de forma correcta ya que no necesariamente un dolar equivale a un real. Para realizar tal operación de sumatoria entre monedas diferentes es necesario primero realizar una cotización de la moneda. El tipo de cambio es la relación de las monedas entre dos paises que resulta en el precio de una de ellas con relación a otra. 