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



