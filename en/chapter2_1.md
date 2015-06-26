### Source code and Installation

The API provides a reference implementation which will be the base to all future implementations. On JSR 354, Money-Api, the reference implementation is **Moneta**. For more information about the project see [https://github.com/JavaMoney/jsr354-ri](https://github.com/JavaMoney/jsr354-ri).

---

####Installing the Project

You could either add it to your project as a dependency from the maven repository or install it from source. 

The maven repo could be found here:
[http://mvnrepository.com/artifact/org.javamoney/moneta](http://mvnrepository.com/artifact/org.javamoney/moneta).



---


#####Adding the dependencies 

Maven

```xml
        <dependency>
            <groupId>org.javamoney</groupId>
            <artifactId>moneta</artifactId>
            <version>moneta_version</version>
        </dependency>
```

Gradle:
```
'org.javamoney:moneta:moneta_version'
```

Ivy:

```
<dependency org="org.javamoney" name="moneta" rev="moneta_version"/>
```

Now you are ready to go and use the API in your project.



---


#####Installing from Source

Another option is to install the project from source. This requires having Git and maven installed as well as Java 8.


Clone the source of  javamoney-parent using the following command:

```
git clone https://github.com/JavaMoney/javamoney-parent.git
```

Switch to the containing folder:

```
cd javamoney-parent
```

Compile the source and install it, the tests will be skipped in this case:

```
mvn clean install -Dmaven.test.skip
```

Done! The next step will be to install **Moneta** from source.

Clone Moneta's source to your local machine:

```
git clone git@github.com:JavaMoney/jsr354-ri.git
```

Switch to the containing folder:
```
cd jsr354-ri
```

Compile the source and install it, the tests will be skipped in this case:

```
mvn clean install -Dmaven.test.skip
```
Done, the code was compiled and installed on local repository. On this book Done, the code was compiled and installed on your local machine. In this book we will be using the master from Github repository, so we recommend  installing locally. The source of  samples used in this book is available on Github [https://github.com/otaviojava/money-api-book-samples](https://github.com/otaviojava/money-api-book-samples).



