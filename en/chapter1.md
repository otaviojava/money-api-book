## The Motivation behind the API

According to Wikipedia, *Money is any item or verifiable record that is generally accepted as payment for goods and services and repayment of debts in a particular country or socio-economic context*. Money is represented by two parts: A numerical value and a currency. We clearly deal with money in our programs everyday, but the JDK doesn't provide a standard representation of money. What we need to know as developers is what data type is suitable to represent  money.

The first attempt would be to use the primitive floating point types (`double`and `float`) that are available in the language. The author of *Effective Java*  doesn't recommend using these types when precise values are required.
 
``` java
double val = 1.03 - .42;
System.out.println(val); //0.6100000000000001
```

As you can see, the result wasn't something that the user would expect. One might ask, is floating point arithmetic broken in Java? No it's not, but Java uses native floating point types and this how **IEEE-754** floating point numbers work, we can't precisely represent base-10 numbers that we as humans tend to use. `Double` and `float` in Java are double-precision 64-bit IEEE-754 floating point and single-precision 32-bit IEEE-754 floating point respectively. 

The same book mentions two ways for dealing with money: 

The first is using the a ``long`` and  an``int`` , but this requires converting the value to its lower primitive units (ex: cents). This solution is highly recommended when performance is an issue. However it is important to worry about the number of decimal places, the book does not recommend greater representation than nine decimal places.

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

One problem with this solution is readability and the other is flexibility. For example, we have to use the value `1200 cents` to represent a product that costs `12 USD`. 

``` java
public class Product {
     private String name;
     private int money;
     //getter and setter
}
Product banana = new Product("banana", 12_00);
Product pasta = new Product("pasta", 4_00);
int sum = banana.getMoney() + pasta.getMoney();
```

It is very easy to go wrong with this design where we could easily forget the fact that we have to convert dollars into cents...


Besides the use of ``int`` and ``long`` Effective Java recommends using ``BigDecimal`` and this makes our lives easier  because it sounds  more natural to say that a product is worth twelve dollars rather than twelve hundred cents.

``` java
public class Product {
     private String name;
     private BigDecimal money;
     //getter and setter
}
Product banana = new Product("banana", BigDecimal.valueOf(12D));
Product pasta = new Product("pasta", BigDecimal.valueOf(4D));
BigDecimal sum = banana.getMoney().add(paste.getMoney());
```

Things are getting better, but there is a very important factor missing in our design, which is currency. If we our program deals with a single currency then we are totally fine, however, this is not the case most of the time. Therefore, the number 12 has no meaning without a currency. 

So lets add a field of type ``String`` to hold the value of the currency.


``` java
public class Product {
     private String name;
     private String currency;
     private BigDecimal money;
     //getter and setter
}
```

Well, this is clearly not a good design because it's not **type-safe**. The `String` is not validated and could be anything that is not a valid currency.

Lets make it type-safe by introducing an ``enum`` of currencies. However, we need to keep various aspects of internationalisation, like  **ISO-4217**, in mind.


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

There is something similar available in the JDK called *java.util.Currency*
that works with **ISO-4217** and solves these two problems:

* It is simple,  we just need to provide  a **Currency**.
* This type supports  ISO 4217.


``` java
public class Product {
     private String name;
     private Currency currency;
     private BigDecimal value;
     //getter and setter
}
```

Validation is essential here when we do arithmetics like sums or discounts. For example we can't sum up prices of products that have different currencies.


``` java
Product banana = //instance;
Product pasta = //instance;
if(banana.getCurrency().equals(pasta.getCurrency())) {
  BigDecimal value = cellular.getValue().add(notebook.getValue());
 }//exception
```

We could introduce a utility class that is responsible for this sort of validation.


``` java
public class ProductUtils {
public static BigDecimal sum(Product pA, Product pB) {
    if(pA.getCurrency().equals(pB.getCurrency())) {
      return pA.getValue().add(pB.getValue());
    }
    throw new IllegalArgumentException("Currency mismatch");
   }

}
BigDecimal sum = ProductUtils.sum(pasta, banana);
```



So would this solve all our problems? No it wouldn't, too many things could go wrong here.

 
* We have to force ourselves and our colleagues  to always use the utility class, and never forget to do so.
* We have to define utility classes for different services or introduce a general  abstraction.

``` java
public class MoneyUtils {
public BigDecimal sum(Currency currencyA, BigDecimal valueA, Currency currencyB, BigDecimal valueB) {
   //...
}
public class ServiceUtils {
...
}
public class WorkerUtils {
...
}
```

So adding an abstraction for representing money becomes more and more obvious after we try all these tricks. Martin Fowler wrote an article describing an abstraction for representing money that solves the following issues:

* Single responsibility , the money class is the only type that is responsible for dealing with money.
* No need for the utility classes because the money class will be the only class responsible for this kind of validation.

``` java
public class Money {
   private  Currency currency;
   private  BigDecimal value;
   //behaviour goes here ...
}
Product banana = new Product("banana", new Money(12, dollar));
Product pasta = new Product("pasta", new Money(4, dollar))
Money money = banana.getMoney().add(pineapple.getMoney());
```


This was the main motivation behind introducing a specification for a standard **API** that abstracts the way we deal with money in our Java programs.