## The Motivation behind the API

According to Wikipedia, *Money is any item or verifiable record that is generally accepted as payment for goods and services and repayment of debts in a particular country or socio-economic context*. Money is represented by two parts: A numerical value and a currency. We clearly deal with money in our programs everyday, but the JDK doesn't provide a standard representation of money. What we need to know as developers is what data type is suitable to represent  money.

So lets try using ``double``. We have a product that costs \$1.03 and after applying some discounts of value \$0.42 the final price is \$0.61.   

``` java
double val = 1.03 - .42;
System.out.println(val); //0.6100000000000001
```
As you can see, the result wasn't something that the user would expect. One might ask, is floating point arithmetic broken in Java? No it's not, but Java uses native floating point types and this how **IEEE-754** floating point numbers work, we can't precisely represent base-10 numbers that we as humans tend to use.

The special thing about money is that its values are discrete. Unlike mass for instance where the value is continuous. In other words, we don't measure money, we count it, and hence exact values are required. 

So how do we fix it? 

We could  use a ``long`` and an ``int`` where it is necessary to perform the conversion value for cents. This solution works really well when performance is an issue. However, with this solution we need to take care of decimal places.

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

One problem with this solution is readability and the other is flexibility. For example, we have to use the value 1200 cents to represent a product that costs 12 USD 
``` java
public class Product {
     private final String name;
     private final int price;
     
     public Product(String name,int price){
       this.name = name;
       this.price = price; 
     }
     //getters ..
}
Product banana = new Product("banana", 12_00);
Product pasta = new Product("pasta", 4_00);
int sum = banana.getMoney() + pasta.getMoney();
```

It is very easy to go wrong with this design where we could easily forget the fact that we have to convert dollars into cents..

The author of **Effective Java** recommends  ``BigDecimal``for representing money. This would make our design easier.

``` java
public class Product {
     private final String name;
     private final BigDecimal price;
     
     public Product(String name,BigDecimal price){
       this.name = name;
       this.price = price; 
     }
     //getters..
}
Product banana = new Product("banana", BigDecimal.valueOf(12D));
Product pasta = new Product("pasta", BigDecimal.valueOf(4D));
BigDecimal sum = banana.getMoney().add(paste.getMoney());
```
Things are getting better, but there is a very important factor missing in our design which is currency. If we our program deals with a single currency then we are totally fine, however, this is not the case most of the time. Therefore, the number 12 has no meaning without a currency. 

So lets add a field of type ``String`` to hold the value of the currency.

``` java
public class Product {
     private final String name;
     private final String currency;
     private final BigDecimal price;
     //getters ...
}
```
Well, this is clearly not a good design because it's not type-safe. The String is not validated and could be anything that is not a valid currency.

Lets make it type-safe by introducing an ``enum`` of currencies. However, we need to keep various aspects of internationalisation, like  **ISO-4217**, in mind.

``` java
public class Product {
     private final String name;
     private final Currency currency;
     private final BigDecimal price;
     //getters..
}
enum Currency {
      REAL, DOLLAR, EURO;
}
```
There is something similar available in the JDK called *java.util.Currency*
that works with **ISO-4217**.

``` java
public class Product {
     private final String name;
     private final Currency currency;
     private final BigDecimal price;
     //getters..
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
..
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

This was the main motivation behind introducing a specification for a standard API that abstracts the way we deal with money in our Java programs. 