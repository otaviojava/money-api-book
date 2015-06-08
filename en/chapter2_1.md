### Acesso ao código fonte e instalação

Como toda especificação no Java, JSR, além da API ela possui uma implementação de referência, como o próprio nome diz, essa implementação servirá de base para as próximas implementações. No caso da JSR 354, money-api, a implementação de referência é o moneta (Para mais informações em relação ao código-fonte do projeto acesse [https://github.com/JavaMoney/jsr354-ri](https://github.com/JavaMoney/jsr354-ri)). Basicamente, é possível utilizar o moneta de duas formas:

A primeira delas é acessando o seu repositório de dependência do maven  ([http://mvnrepository.com/artifact/org.javamoney/moneta](http://mvnrepository.com/artifact/org.javamoney/moneta)), assim caso o seu projeto use o maven, por exemplo, basta adicionar a dependência do moneta em seu projeto da seguinte forma:

```
        <dependency>
            <groupId>org.javamoney</groupId>
            <artifactId>moneta</artifactId>
            <version>moneta_version</version>
        </dependency>
```

Caso seja Gradle:
```
'org.javamoney:moneta:moneta_version'
```

Caso seja Yvi:
```
<dependency org="org.javamoney" name="moneta" rev="moneta_version"/>
```


A outra forma é baixando o código-fonte e o compilando, para isso será necessário seguir alguns passos, mas basicamente é baixar o parent o instalá-lo e em seguida instalar o moneta. Vale lembrar que para realizar esse procedimento é necessário ter o git, Java 8 e o maven instalado e configurado em sua máquina.


Baixar o código do javamoney-parent, para isso basta executar o seguinte comando:

```
git clone https://github.com/JavaMoney/javamoney-parent.git
```

Em seguida acesse a pasta:
```
cd javamoney-parent
```

Compile e código-fonte e o instale em seu repositório local, no nosso caso também pularemos os testes apenas para que seja mais rápido:

```
mvn clean install -Dmaven.test.skip
```

Pronto, uma vez feito isso o próximo passo será realizar a instalação do moneta.

Baixar o código do moneta, para isso basta executar o seguinte comando:

```
git clone git@github.com:JavaMoney/jsr354-ri.git
```

Em seguida acesse a pasta:

```
cd jsr354-ri
```

Compile e código-fonte e o instale em seu repositório local, no nosso caso também pularemos os testes apenas para que seja mais rápido:

```
mvn clean install -Dmaven.test.skip
```


Pronto, código-fonte instalado e poderá ser utilizado tranquilamente. No caso do livro estamos usando como base a master do repositório, ou seja, é recomendável que baixe o código-fonte e o instale localmente. Para ter acesso ao código-fonte de exemplo basta acessar:

* [https://github.com/otaviojava/money-api-book-samples](https://github.com/otaviojava/money-api-book-samples)

