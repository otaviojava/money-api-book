# Introducción

Buena parte de los sistemas de información que desarrollamos manipulan valores monetarios y monedas. Sin embargo, es en lo minimo curioso percibir que los conceptos de "Dinero" y "Moneda" comunmente son desplazados a tipos primitivos como Float, Double y BigDecimal. Sistemas mejor encapsulados terminan implementando cada uno en sus propias clases de "Dinero" y "Moneda". Pero hasta cuando los desarrolladores Java tendrian que continuar resolviendo los mismos problemas sin tener una solución de referencia standard?

La especificación JSR 354 (Money and Currency API) es un esfuerzo para definir un API y proveer una implementación de referencia para solucionar los problemas definidos de acuerdo a los conceptos de "Dinero" y "Moneda".
