# Una especificación de Dinero, realmente vale la pena?

Dinero es la forma mas común de realizar intercambio de bienes, compra de materiales, etc. Este concepto fué y ha sido representado en diversos programas que usan Java. Pero despues de todo cual es la motivación detrás de usar un tipo Dinero? Acaso ya no vale la pena mas usar un tipo primitivo de Java como ```Double```, ```Float```, ```String```, ```BigDecimal```, etc.? Existen mejores soluciones que esa? Que sucede con mi encapsulación cuando se toma la decisión de usar tipos primitivos? Usar clases Helper para tratar la misma moneda resulta interesante, pero si el desarrollador obvia u olvida de usar esa clase Helper puede ser contraproducente. Una vez que sea necesario crear un tipo Dinero, emerge la primera duda: Será que nadie mas pasó por este problema antes? Recuerde que el factor de quedar "reinventando la rueda" no es una buena estratégia sin hablar, que el desarrollador pasará por los mismos problemas ya resueltos por otros mas experimentados. Con ese objetivo nació la especificación Moneda.

La JSR 354, es un especificación Java cuyo objetivo es tomar cuenta del dinero y resolver algunos problemas triviales que los desarrolladores Java van enfrentando en su dia a dia. A partir de esa especificación será mas facil trabajar con dinero de forma estandar, ya con la era de los microservicios será mas facil lidiar con comunicaciones de este tipo, por ejemplo solicitar un pago de un producto en e-commerce a partir de una pasarela de pago. Ese material estará dividido en algunas partes:

En el primer capítulo se discutirá la motivación para utilizar un tipo Dinero en su sistema, los beneficios tanto en diseño, centralización de código, mantenimiento y orientación a objetos.

Creación de un tipo Dinero, pero será que nunca nadie tuvo ese problema? esa pregunta será contestada en el segundo capítulo, hablando de la motivación de la especificación junto con el uso básico del API.

Extraer valores y realizar pequeñas operaciones a partir de un valor monetário, ese es el principal objetivo de las operaciones como query y operador: Será demostrando el funcionamiento de las clases ```MonetaryOperator``` y ```MonetaryQuery```, diferencias entre ellas, las clases utilitarias ```MonetaryOperators``` y ```MonetaryQueries``` y como crear una query u operación en el Dinero.

Dar formato a una cantidad monetaria: Será demostrado aqui la forma de exhibir el formato de dinero con clases ya soportadas por la API, incluso de como crear un formateador para el cero.

Si, ahora estamos preparados para Java 8!: Conozca las funciones construidas ya existentes en las implementaciones hechas para trabajar con Streams de sumas monetarias.

Convertiendo valores: Que sucede cuando probamos realizar sumas de dinero con monedas diferentes? Claramente lanzará una excepción verdad? En este punto será demostrado como realizar conversión con cotizaciones de valores, además de realizar la busqueda de cotización a partir de datos especificos.

Trabajando con Java EE: Convertidores de JSF, CDI y Spring, en este capítulo mostraremos clases utilitarias para trabajar con esas tecnologias.

Esperamos que los lectores gusten bastante del proyecto realizado en conjunto con toda la comunidad Java.