## A motivação do tipo dinheiro


Segundo o Wikipédia, o dinheiro é o meio usado na troca de bens, usado na compra de bens, serviços, força de trabalho, divisas estrangeiras ou nas demais transações financeiras, emitido e controlado pelo governo de cada país, que é o único que tem essa atribuição. Considerando isso, muitos sistemas em Java acabam utilizando ou representando esse o valor monetário, mas como representar o dinheiro em seu sistema?

Para representar o dinheiro a primeira estratégia é utilizar os tipos já oriundos do Java, o livro Java Efetivo não recomenda a utilização do uso do ``double`` e ``float`` quando respostas precisas são necessárias. 

``` java
double val = 1.03 - .42;
System.out.println(val); //0.6100000000000001
```

Esse mesmo livro recomenda duas estratégias, a primeira delas é utilizando o long e o ``int``, para isso, é necessário realizar a conversão do valor para centavos, essa solução é muito recomendada quando a velocidade e a ocupação de memória são pontos importantes, no entanto, é importante se preocupar com o número de casas decimais, o livro não recomenda representação maior que nove casas decimais.

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

Um problema de utilizar a representação do dinheiro com ``int`` e ``long`` é a dificuldade e a não legibilidade de representar valores monetários dessa forma. Para exemplificar, um produto tem um preço no valor de doze dólares, como representamos em centavos, colocaremos o valor de mil e duzentos centavos.

``` java
public class Product {
     private String name;
     private int money;
     //getter and setter
}
Produto banana = new Produto("banana", 12_00);
Produto pasta = new Produto("pasta", 4_00);
int sum = banana.getMoney() + macarrao.getMoney();
```

 Mas o que aconteceria se esquecermos de converter esse valor de dólares para centavos (no caso colocar apenas doze em vez de mil e duzentos)? Certamente o resultado seria desastroso, outro problema estaria no controle de arredondamentos.


Além do uso de ``int`` e ``long`` o Java efetivo recomenda o uso do ``BigDecimal``, com isso, nosso produto terá uma chamada bem mais intuitiva e mais comum, afinal, é mais natural falar que um produto custa doze dólares e não mil e duzentos centavos.

``` java
public class Product {
     private String name;
     private BigDecimal money;
     //getter and setter
}
Produto banana = new Produto("banana", BigDecimal.valueOf(12D));
Produto pasta = new Produto("pasta", BigDecimal.valueOf(4D));
BigDecimal sum = banana.getMoney().add(macarrao.getMoney());
```

Outro ponto importante que o ``BigDecimal`` já trata é o controle de arredondamentos de maneira tranquila.

Indo além com a nossa classe produto, temos um pequeno problema com ela, não representamos a moeda! Ou seja, ela ficou subentendida em todos os casos, caso o meu sistema lide apenas com uma moeda isto não é um problema, mas imagine que o meu produto seja vendido em diversos pontos do mundo. Apenas o doze não significa nada, doze pode ser qualquer coisa (reais, pesos, dólares, etc.).

Para representar o dinheiro é importante entendê-lo. De forma resumida, o dinheiro é composto por duas partes, a parte do valor que é a quantidade numérica, mas apenas com esse valor não conseguimos fazer muita coisa, precisamos da moeda. A moeda representa o “sistema do dinheiro” em comum uso, especialmente dentro de uma nação, seguindo essa definição o real, peso, dólar e euros são tipos de moedas. Portanto, teremos que adicionar a moeda dentro do produto. Podemos representar moeda de algumas formas: 

A primeira delas é utilizando o tipo ``String``, mas o que acontece se em vez de escrever dólar escrever “dolra” com um pequeno problema de escrita? Não temos nenhum controle com o tipo String, assim ele pode receber desde um pequeno erro de escrita até valores ilógicos como banana, macarrão, etc. Apesar destes últimos não serem moedas serão normalmente aceitos se forem passados como ``String``.

``` java
public class Product {
     private String name;
     private String currency;
     private BigDecimal money;
     //getter and setter
}
```

A segunda estratégia seria utilizar um ``enum`` para representar as moedas, dessa forma, as opções serão restritas. Com essa estratégia resolvemos o problema da ~~``String``,~~ apenas serão possíveis os valores que definiremos a partir do ``enum``, porém nosso ``enum`` precisará ficar mais rico uma vez que temos de lidar com diversos aspectos de internacionalização dentre eles a ISO *4217*, padrão para moeda.


``` java
public class Product {
     private String name;
     private Currency currency;
     private BigDecimal value;
     //getter and setter
}
enum Currency {
    	REAL, DOLLAR, EURO;
}
```

Para resolver isso, é possível utilizar uma classe já existente dentro do JDK a classe *java.util.Currency*, com ela conseguimos resolver os dois problemas:

* Apenas entrarão valores do tipo **Currency** no setter.
* Essa classe já trabalha com a ISO 4217.


``` java
public class Product {
     private String name;
     private Currency currency;
     private BigDecimal value;
     //getter and setter
}
```

Com o nosso dinheiro composto precismos validar que as moedas são as mesmas na hora de realizar a compra ou realizar o somatório, afinal, a cotação de um produto em real deve ser diferente de um em dólar.

``` java
Product banana = //instance;
Product pasta = //instance; 	
if(banana.getCurrency().equals(pasta.getCurrency())) {
  BigDecimal value = celular.getValue().add(notebook.getValue());
 }//exception
```

Possivelmente teremos que realizar essa validação em diversos pontos do nosso código, dessa forma criaremos uma classe utilitária.


``` java
public class ProductUtils {
public static BigDecimal sum(Product pA, Product pB) {
    if(pA.getCurrency().equals(pB.getCurrency())) {
      return pA.getValue().add(pB.getValue());
    }
    throw new IllegalArgumentException("Currency mismatch");
   }
}
BigDecimal sum = ProdutoUtils.sum(pasta, banana);
```



Pronto, Com isso resolvemos todos os nossos problemas, certo? Errado! Vamos listar alguns possíveis problemas:

 
* Para realizar o somatório de produtos é necessário que a pessoa lembre de realizar a chamada da classe utilitária, mas o que acontece com aquilo que você tem de lembrar? Exato, fatalmente se esquece. 
* Como falamos acima, o dinheiro pode ser usado não apenas com produto, mas com diversas coisas, serviços, força de trabalho, etc., assim será necessário duplicar os dois campos, moeda e valor monetário, para diversos pontos.
* Uma vez com diversas classes utilizando o dinheiro teremos duas estratégias para realizar a validação, uma seria criar classes utilitárias para todo modelo que use dinheiro, ServiceUtils, GoodsUtils, etc., ou uma classe utilitária que recebe quatro parâmetros (o valor e a moeda dos dois para ser comparado e então somado).

``` java
public class MoneyUtils {
public static BigDecimal sum(Currency currencyA, BigDecimal valueA, Currency currencyB, BigDecimal valueB) {
   //...
}
public class ServiceUtils {}
public class WorkerUtils {}
```

* O que acontece se eu apenas definir apenas um único item do dinheiro, o valor ou a moeda? Faz sentido dizer que o produto vale doze? Ou que ele vale dólar? Absolutamente não, ele vale doze dólares e isso precisa ser validado.
* É de responsabilidade da classe produto, ou qualquer outra que precise trabalhar com o dinheiro, cuidar da criação e do estado do dinheiro? 
* Uma vez utilizando classes utilitárias para realizar essa validação não estamos vazando encapsulamento? Afinal é possível realizar o somatório de dois valores ignorando a validação da moeda gerando erro. Olhando a definição do Wikipédia sobre o encapsulamento: Permite esconder propriedades e métodos de um objeto para proteger o código de corrupções acidentais.


Além desses problemas, usando como referência o Clean Code, temos uma ótima definição entre estrutura de dados e um objeto, basicamente o objeto esconde os dados para expor um comportamento, ou seja, não estamos programando orientado a objetos dessa forma.

A solução para resolver esse problema virá de um artigo do Martin Fowler, na qual ele cita o exemplo do dinheiro como o seu favorito, assim será criado o tipo dinheiro. Com isso resolveremos:

* Centralização do código, todo o comportamento do dinheiro estará na classe dinheiro.
* Removeremos a responsabilidade das outras classes, não será necessário, por exemplo, ter o controle na hora de criar valores dentro da classe produto citada anteriormente.
* Adeus as classes utilitárias, uma vez a validação dentro da classe dinheiro, as classes utilitárias não serão mais necessárias, sem falar no clássico problema de esquecer de usá-las. 

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


Com isso se trouxe a motivação por trás da criação do tipo dinheiro. Além de evitar problemas, por exemplo, o esquecimento da validação do dinheiro, código espelhado e desencapsulado garantimos também maior qualidade de código como responsabilidade única, dinheiro como objeto e não apenas como estrutura de dados e trazemos o dinheiro para o domínio da nossa aplicação.
