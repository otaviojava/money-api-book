# Uma especificação de dinheiro, será que vale mesmo a pena?


Dinheiro é a forma mais comum de realizar trocas de bens, compra de materiais etc. Ele certamente foi, é e será representando em diversos programas que rodam Java. Mas afinal qual é a motivação por trás de se usar um tipo dinheiro? Não vale mais a pena utilizar um tipo primitivo do Java como o ```Double```, ```Float```, ```String```, ```BigDecimal```, etc.? Existem soluções melhores que essa? O que acontece com o meu encapsulamento quando se toma decisão de usar tipos primitivos? Usar classes utilitárias para tratar a mesma moeda pode ser muito interessante, mas caso a desenvolvedor esqueça de utilizar tal classe o resultado poderá ser desastroso. Uma vez que seja necessário criar um tipo dinheiro, surge a primeira dúvida: Será que ninguém nunca passou por esse problema antes? Vale lembrar que o fator de ficar “reinventando a roda” não é uma boa estratégia sem falar, que o desenvolvedor passará pelos mesmos problemas já resolvidos por outros mais experientes. Com esse intuito nasceu a especificação de moeda.


A JSR 354, é uma especificação Java cujo o objetivo é tomar conta do dinheiro e resolvendo alguns problemas triviais que os desenvolvedores Java vem enfrentado em seu dia a dia. A partir dessa especificação será mais fácil trabalhar com dinheiro de forma padronizada, já que com a era dos micros serviços será mais fácil lidar com a comunicação desse tipo, por exemplo, um e-commerce solicitar um pagamento de um produto a partir de um gateway de pagamento. Esse material será dividido em algumas partes:

No primeiro capítulo será discutido a motivação por trás de utilizar um tipo dinheiro em seu sistema, os benefícios tanto no design, centralização de código, manutenção e orientação a objetos.

Criação de um tipo dinheiro, mas será que ninguém nunca teve esse problema? Essa pergunta será respondida nesse segundo capítulo, falando da motivação da especificação além do uso básico da API.

Extrair valores e realizar pequenas operações a partir de um valor monetário, esse é o principal objetivo das operações com query e operador: Será demonstrado o funcionamento das classes ```MonetaryOperator``` e ```MonetaryQuery```, diferenças entre elas, as classes utilitárias ```MonetaryOperators``` e ```MonetaryQueries``` e como criar uma query ou operação no dinheiro.

Formatando um montante monetário: Será demonstrado aqui a forma de exibir formatar o dinheiro com classes já suportadas pela API, além de como criar um formatador do zero.

Sim, estamos preparados para o Java 8!: Conheça as funções embutidas dentro da RI feita para trabalhar com **Streams** de montante monetário.
Convertendo valores: O que acontece quando tentamos realizar somatórios de dinheiros com moedas diferentes? Certamente lançará uma exceção certo? Nesse ponto será demonstrado como realizar conversão com cotações de valores, além de realizar a busca de cotação a partir de uma data específica.
 Trabalhando com o Java EE: Conversores de JSF, CDI e Spring, nesse capítulo mostraremos classes utilitárias para trabalhar com essas tecnologias.

Espero que os leitores curtam bastante o projeto realizado em conjunto com toda a comunidade Java.