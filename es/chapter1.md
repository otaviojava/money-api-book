## La motivación del tipo Dinero


Según Wikipedia, el dinero es el medio usado en el intercambio de bienes, usado en la compra de bienes, servicios, fuerza de trabajo, divisas extranjeras o en las demás transacciones financieras, emitido y controlado por el gobierno de cada país, que es el unico que tiene esa atribución. Considerando eso, muchos sistemas en Java terminan utilizando o representando ese valor monetário, pero como representar el dinero en su sistema?

Para representar el dinero la primera estratégia es utilizar los tipos nativos de Java, el libro Java Efectivo no recomienda la utilización del uso de ``double`` y ``float`` cuando resultados precisos son necesarios. 

``` java
double val = 1.03 - .42;
System.out.println(val); //0.6100000000000001
```

Ese mismo libro recomienda dos estratégias, la primera de ellas es utilizando long e ``int``, para esto, es necesario realizar una conversión de valor para centavos, esta solución es muy recomendada cuando la velocidad y la ocupación de memoria son puntos importantes, sin embargo, es importante preocuparse con el numero de lugares decimales, el libro no recomienda representación mayor que nueve lugares decimales.

``` java
public static void main(String[] args) {
int itemsBought = 0;
int funds = 100;
for (int price = 10; funds >= price; price += 10) {
itemsBought++;
funds -= price;
}
System.out.println(itemsBought + " items bought.");
System.out.println("Money left over: "+ funds + " cents");
}
```

El problema de utilizar la representación de dinero con ``int`` y ``long`` es la dificultad y la no legibilidad de representar valores monetarios de esa forma. Para dar un ejemplo, un producto tiene un precio de doce dolares de valor, como representamos en centavos, colocaremos el valor de mil docientos centavos.

``` java
public class Producto {
     private String name;
     private int money;
     //getter and setter
}
Producto banana = new Producto("banana", 12_00);
Producto pasta = new Producto("pasta", 4_00);
int sum = banana.getMoney() + pasta.getMoney();
```

Pero que sucede si olvidamos convertir ese valor de dolares para centavos (es decir, dejar doce en vez de poner doce mil)? aqui el resultado seria erroneo, otro problema seria el control de redondeos de decimales.


Además de ``int`` y ``long`` el libro Java Efectivo recomienda el uso de ``BigDecimal``, con eso, nuestro producto tendrá una llamada más intuitiva y mas común, despues de todo, es mas natural hablar de un producto que cuesta doce dolares y no mil doscientos centavos.

``` java
public class Producto {
     private String name;
     private BigDecimal money;
     //getter and setter
}
Producto banana = new Producto("banana", BigDecimal.valueOf(12D));
Producto pasta = new Producto("pasta", BigDecimal.valueOf(4D));
BigDecimal sum = banana.getMoney().add(macarrao.getMoney());
```

Otro punto importante es que ``BigDecimal`` ya trata el control de redondeo de decimales de manera mas tranquila.

Al ir mas allá con nuestra clase Producto, tenemos un pequeño problema con ella, no representamos la moneda! O sea, esta quedó implicita en todos los casos, si mi sistema afronta apenas con una moneda no es un problema, pero imagine que mi producto sea vendido en diversos lugares del mundo. Solo el doce no significa nada, doce puede ser cualquier cosa (reales, soles, pesos, dólares, etc.).

Para representar el dinero es importante entenderlo. De forma resumida, el dinero está compuesto por dos partes, la parte del valor que es una cantidad numérica, pero solo con ese valor no conseguimos hacer mucha cosa, necesitamos de la moneda. La moneda representa el "sistema de dinero" de comun uso especialmente dentro de una nación, siguiendo esa definición el real, nuevo sol, peso, dolar y el euro son tipos de monedas. Por lo tanto, tendremos que adicionar una moneda dentro del producto. Podemos representar la moneda de varias formas: 

La primera de ellas es utilizando el tipo ``String``, pero que sucede si en vez de escribir dólar se escribe “dolra” con un pequeño problema de escritura? no tenemos ningun control con el tipo String, asi puede recibir un pequeño error de escritura hasta valores ilogicos como banana, pasta, etc. Estos ultimos no serian monedas, pero serán aceptados normalmente pasados como ``String``.

``` java
public class Producto {
     private String name;
     private String currency;
     private BigDecimal money;
     //getter and setter
}
```

La segunda estratégia seria utilizar un ``enum`` para representar las monedas, de esa forma, las opciones serán estrictas. Con esta estratégia resolvemos el problema de ~~``String``,~~ solo serán posibles los valores que definiremos a partir de ``enum``, sin embargo nuestro ``enum`` necesitará quedar mas detallado una vez que tengamos que enfrentar aspectos de internacionalización, dentro de esos la ISO *4217*, un estandar para monedas.


``` java
public class Producto {
     private String name;
     private Currency currency;
     private BigDecimal value;
     //getter and setter
}
enum Currency {
    	REAL, DOLLAR, EURO;
}
```

Para resolver esto, es posible utilizar una clase ya existente dentro del JDK: la clase *java.util.Currency*, con esta clase conseguimos resolver los dos poblemas:

* Solamente entrarán valores de tipo **Currency** en los setter.
* Esta clase ya trabaja con la ISO 4217.


``` java
public class Producto {
     private String name;
     private Currency currency;
     private BigDecimal value;
     //getter and setter
}
```

Con el manejo de dinero de distintos tipos, necesitamos validar si las monedas son las mismas a la hora de realizar la compra o hacer una suma, despues de todo, la tipo de cambio de un producto en real debe ser distinto de uno en dolar.

``` java
Producto banana = //instance;
Producto pasta = //instance; 	
if(banana.getCurrency().equals(pasta.getCurrency())) {
  BigDecimal value = celular.getValue().add(notebook.getValue());
 }//exception
```

Posiblemente tendremos que realizar esa validación en diversos lugares de nuestro código, de esta forma crearemos una clase utilitaria.


``` java
public class ProductoUtils {
public static BigDecimal sum(Producto pA, Producto pB) {
    		if(pA.getCurrency().equals(pB.getCurrency())) {
return pA.getValue().add(pB.getValue());
  	}
return null;
    	}
}
BigDecimal sum = ProductoUtils.sum(pasta, banana);
```


Listo, con esto resolvemos todos nuestros problemas, correcto?, Falso!, vamos a nombrar algunos posibles problemas:

 
* Para realizar la sumatoria de productos es necesario que la persona se acuerde de realizar la llamada a la clase utilitaria, pero que pasa con aquello que tiene que acordarse? Exacto!, desafortunadamente se olvida. 
* Como hablamos anteriormente, el dinero puede ser usado no solamente con Producto, con diversas cosas tambien, servicios, fuerza de trabajo, etc., asi será necesario duplicar los dos campos, moneda y valor monetário, para diversos lugares.
* Desde diversas clases utilizando dinero tendremos dos estratégias para realizar la validación, una seria crear clases utilitarias para todo modelo que use dinero, como  ServiceUtils, GoodsUtils, etc., o una clase utilitaria que recibe cuatro parametros (el valor de la moneda de los dos para ser comparado y entonces sumado).

``` java
public class MoneyUtils {
public static BigDecimal sum(Currency currencyA, BigDecimal valueA, Currency currencyB, BigDecimal valueB) {
   //...
}
public class ServiceUtils {}
public class WorkerUtils {}
```

* Que pasa si solo definimos un unico item de dinero, valor o moneda? Tiene sentido decir que el producto vale doce? o que vale dólar? Definitivamente no, este vale doce dólares y eso necesita ser validado.
* Es de responsabilidad de la clase producto, o cualquier otra que necesite trabajar con dinero, cuidar de la creación y del estado de dinero? 
* Una vez utilizado clases utilitarias para realizar esa validación no estamos aminorando encapsulamiento? Despues de todo es posible realizar la sumatoria de dos valores ignorando la validación de la moneda generando asi un error. Mirando la definición de Wikipédia sobre el encapsulamiento: Permite esconder propiedades y métodos de un objeto para proteger el código de acceso directos y efectos secundarios accidentales.


Además de esos problemas, usando como referencia el libro Clean Code, tenemos una optima definición entre estructura de datos y un objeto, basicamente el objeto esconde los datos para exponer un comportamiento, o sea, no estamos programando orientado a objetos de esa forma.

Una solución para resolver ese problema vendrá de un articulo de Martin Fowler, en el cual el cita un ejemplo de tipo dinero como su favorito, asi será creado el tipo dinero. con eso resolveremos:

* Centralización de código, todo el comportamiento de dinero estará en la clase dinero.
* Eliminaremos la responsabilidad de las otras clases, no será necesario, por ejemplo, tener el control a la hora de crear valores dentro de la clase Producto citada anteriormente.
* Adiós a las clases utilitarias, una vez que la validación dentro de la clase dinero, las clases utilitarias no serán mas necesarias, para ya no pensar en el clasico problema de olvidarnos de usarlas. 

``` java
public class Money {
   private  Currency currency;
   private  BigDecimal value;
   //behavior here
}
Product banana = new Product("banana", new Money(12, dollar));
Product pasta = new Product("pasta", new Money(4, dollar))
Money money = banana.getMoney().add(abacaxi.getMoney());
```


Con eso se trajo la motivación para la creación de un API de tipo dinero. Además de evitar problemas, por ejemplo, de olvidarse de validar dinero, codigo verboso y desencapsulado se garantiza mayor calidad de código como responsabilidad única, dinero como objeto y no solo como estructura de datos, traemos dinero para el dominio de nuestra aplicación como un API.
