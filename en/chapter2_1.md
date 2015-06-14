### Access source and install

How all specification on Java, JSR, beyond the  API it has a reference implementation, this implementation will serve as base to all next implementations. On JSR 354, money-api, the reference implementation is the Moneta (To access more information the source of the project see [https://github.com/JavaMoney/jsr354-ri](https://github.com/JavaMoney/jsr354-ri)). Basically, is possible to use the money of two ways:

The first one is accessing the dependency's repository from maven ([http://mvnrepository.com/artifact/org.javamoney/moneta](http://mvnrepository.com/artifact/org.javamoney/moneta)), so if the project use the maven, for example, just adds a Moneta's dependency on project.


```xml
        <dependency>
            <groupId>org.javamoney</groupId>
            <artifactId>moneta</artifactId>
            <version>moneta_version</version>
        </dependency>
```

To gradle:
```
'org.javamoney:moneta:moneta_version'
```

To Yvi:

```
<dependency org="org.javamoney" name="moneta" rev="moneta_version"/>
```


The last one is downloading the source and compiling, for do this, will necessary to follow some steps, to do it just is necessary download and install the parent and install and then install **Moneta**. Just to remember the minimum requisites is git, Java 8 and maven installed and configured on the machine.


Download the source of  javamoney-parent, to do it run this command:

```
git clone https://github.com/JavaMoney/javamoney-parent.git
```

Get in the folder:

```
cd javamoney-parent
```
Compile the source and install, in this case the test will skipped:

```
mvn clean install -Dmaven.test.skip
```

Done, the next step will install the Moneta from source.

Download the source of  Moneta, to do it run this command:

```
git clone git@github.com:JavaMoney/jsr354-ri.git
```

Get in the folder:
```
cd jsr354-ri
```

Compile the source and install, in this case the test will skipped:

```
mvn clean install -Dmaven.test.skip
```
Done, the code was compiled and installed on local repository. On this book will use the master from github repository, so is recommend be installed locally. To get the source used as sample on this cook book access:  [https://github.com/otaviojava/money-api-book-samples](https://github.com/otaviojava/money-api-book-samples).



