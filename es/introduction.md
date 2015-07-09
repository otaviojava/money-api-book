# Una especificación de Dinero, realmente vale la pena?

Dinero es la forma más común de realizar intercambio de bienes, compra de materiales, etc. Este concepto fue y ha sido representado en diversos programas que usan Java. Pero después de todo, ¿Cuál es la motivación detrás de usar un tipo Dinero? ¿Acaso ya no vale la pena usar un tipo de dato primitivo de Java como ```Double```, ```Float```, ```String```, ```BigDecimal```, etc.? ¿Existen mejores soluciones que esa? ¿Qué sucede con mi encapsulación cuando se toma la decisión de usar tipos primitivos? Usar clases Helper para tratar la misma moneda resulta interesante, pero si el desarrollador obvia u olvida de usar esa clase Helper puede ser contraproducente. Una vez que sea necesario crear un tipo Dinero, emerge la primera duda: ¿Será que nadie más pasó por este problema antes? Recuerde que el factor de quedar "reinventando la rueda" no es una buena estratégia, sin hablar que el desarrollador pasará por los mismos problemas ya resueltos por otros más experimentados. Con ese objetivo nació la especificación Dinero y Moneda.

La JSR 354, es un especificación Java cuyo objetivo es el de tomar cuenta del dinero y resolver algunos problemas triviales que los desarrolladores Java van enfrentando en su dia a dia. A partir de esa especificación será más fácil trabajar con dinero de forma estándar, ya con la era de los microservicios será más fácil lidiar con comunicaciones de este tipo, por ejemplo solicitar un pago de un producto en e-commerce a partir de una pasarela de pago. Este material estará dividido en los siguientes temas:

- En el primer capítulo se discutirá la motivación para utilizar el tipo Dinero en su sistema, los beneficios tanto en diseño, centralización de código, mantenimiento y orientación a objetos.

- Creación de un tipo Dinero, pero ¿Será que nunca nadie tuvo ese problema? Esa pregunta será contestada en el segundo capítulo, hablando de la motivación de la especificación junto con el uso básico de la API.

- Extraer valores y realizar operaciones a partir de un valor monetario, ese es el principal objetivo de los tipos **query** y **operator**: Se mostrará el funcionamiento de las clases ```MonetaryOperator``` y ```MonetaryQuery```, sus diferencias, como crear instancias de cada una usando las clases utilitarias ```MonetaryOperators``` y ```MonetaryQueries```.

- Dar formato a un valor monetaria: Aquí se mostrará la forma de dar formato a valores monetarios con clases ya soportadas por la API, incluso mostraremos como crear un formateador desde cero.

- Sí, ahora estamos preparados para Java 8!: Conozca las funciones construidas ya existentes en las implementaciones hechas para trabajar con Streams de valores monetarios.

- Convertiendo valores: ¿Qué sucede cuando queremos realizar sumas de valores de monedas diferentes? Claramente se lanzará una excepción, ¿Verdad? En este punto será demostrado como realizar conversión de valores monetarios entre diferentes monedas por medio de tasas de cambio, además de realizar la busqueda de tasas de cambio a partir de datos especificos.

- Trabajando con Java EE: Convertidores de JSF, CDI y Spring, en este capítulo mostraremos clases utilitarias para trabajar con esas tecnologias.

Esperamos que los lectores gusten bastante del proyecto realizado en conjunto con toda la comunidad Java.
