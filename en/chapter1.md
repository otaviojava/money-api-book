## The motivation behind the money type

According to Wikipedia, money is the means used to exchange goods, used in the purchase of goods, services, workforce, foreign currencies or in other financial transactions, issued and controlled by the government of each country, which is the only one this assignment. Considering this, many Java systems end up having to represent this monetary value. How should you depict it in your system?

The first strategy is to use the types already coming from Java, Java Effective book does not recommend the use of the ``double`` and ``float`` when precise answers are necessary. 

``` java
double val = 1.03 - .42;
System.out.println(val); //0.6100000000000001
```

The same book highlights two strategies for dealing with money: 

The first is using the ``long`` and ``int`` for that, it is necessary to perform the conversion value for cents, this solution is highly recommended when the speed and memory occupation are important points, however it is important to worry about the number of decimal places, the book does not recommend greater representation that nine decimal places

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

A problem to use the representation of money with ``int`` and ``long`` is the readability of representing monetary values that way. As an example, a product has a price in the amount of twelve US dollars, how to represent in cents,  we put the value of twelve hundred cents.

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

 But what would happen if we forget to convert this amount of dollars to cents (in the case  twelve, instead of twelve hundred)? Certainly the result would be disastrous, another problem would be in control of rounding.


Besides the use of ``int`` and ``long`` effective Java recommends using ``BigDecimal``, with that, our product will have a much more intuitive and more common call, after all, it is more natural to say that a product is twelve dollars and not twelve hundred cents.

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

Another important point that the ``BigDecimal`` already addresses quiet way is the rounding control.

Going beyond with our product class, we have a little problem with it, do not represent the currency! That is, it was implied in all cases, if my system only deal with a currency that is not a problem, but imagine that my product is sold in various parts of the world. Only the twelve did not mean anything, twelve can be anything (real, pesos, dollars, etc.).

To represent money is important to understand it. Briefly, the money consists of two parts, the part of the value that is the numeric quantity, but only this amount can not do a lot, we need the money. The currency is the "money system" in common use, especially within a nation, following this definition the real ,weight, dollar and euro are types of currencies. Therefore, we have to add the currency within the product. We can represent currency in some ways: 

The first is using the type ``String``,but what if instead of writing dollar write "dolra" with a little trouble writing? We have no control with the type String, so it can receive from a small clerical error until illogical values such as bananas, pasta, etc. Although the latter are not coins will normally be accepted if they are passed as ``String``.

``` java
public class Product {
     private String name;
     private String currency;
     private BigDecimal money;
     //getter and setter
}
```

The second strategy would be to use a``enum`` to represent the coins, therefore, the options are limited. With this strategy we solve the problem of ~~``String`` ~~, will only be possible to define the values from ``enum``,  but our ``enum`` need to get richer since we have to deal with various aspects of internationalization among them ISO *4217*, standard for currency.


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

To resolve this, you can use an existing class within the JDK the *java.util.Currency* class, she managed to solve two problems:

* Just enter values **Currency** kind in the setter.
* This class has been working with the ISO 4217.


``` java
public class Product {
     private String name;
     private Currency currency;
     private BigDecimal value;
     //getter and setter
}
```

With our money made must validate that the coins are the same time to make a purchase or perform the sum, after all, the price of a real product must be different than a dollar.

``` java
Product banana = //instance;
Product pasta = //instance;
if(banana.getCurrency().equals(pasta.getCurrency())) {
  BigDecimal value = cellular.getValue().add(notebook.getValue());
 }//exception
```

Perhaps we will have to perform a validation in various parts of our code in this way will create a utility class.


``` java
public class ProductUtils {
public static BigDecimal sum(Product pA, Product pB) {
    		if(pA.getCurrency().equals(pB.getCurrency())) {
return pA.getValue().add(pB.getValue());
  	}
return null;
    	}
}
BigDecimal sum = ProdutoUtils.sum(pasta, banana);
```



Ready, With that solve all our problems, right? Wrong! We will list some possible problems:

 
* To perform the product sum is necessary that the person remember to make the call of the utility class, but what about what you have to remember ? Accurate , inevitably forget. 
* As we mentioned above, the money can be used not only with the product but with different things, services, labor, etc., so it will be necessary to duplicate the two camps, currency and monetary value, for several points.
* Once with several classes using the money we have two strategies to perform validation, one would create utility classes for every model that uses money, ServiceUtils, GoodsUtils, etc., or a utility class that receives four parameters (the amount and the currency of two to be compared and then added).

``` java
public class MoneyUtils {
public static BigDecimal sum(Currency currencyA, BigDecimal valueA, Currency currencyB, BigDecimal valueB) {
   //...
}
public class ServiceUtils {}
public class WorkerUtils {}
```

* What happens if I only just set a single item of money, value or currency? It makes sense to say that the product is worth twelve? Or he's worth dollar? Absolutely not, it is worth twelve US dollars and this needs to be validated.
* It is the product liability class, or any that need to work with money, care for creation and the money the state? 
* Once using utility classes to perform this validation we are not leaking package? After all it is possible to the sum of both values ignoring the currency of generating validation error. Looking at the Wikipedia definition of encapsulation: Allows hide properties and methods of an object to protect the code from accidental corruption.

In addition to these problems, using as reference the Clean Code, we have a great definition of data structure and an object, the object basically hides the data to exhibit a behavior, that is, we are not object-oriented programming that way.

The solution to this problem will come from an article by Martin Fowler, in which he cites the example of money as your favorite, so the kind of money is created. With that solve:

* Centralization of code, all the money behavior is the money class.
* Remove the liability of the other classes, you do not need, for example, have control when creating values within the class product mentioned above.
* Goodbye the utility classes, once the validation in the money class, the utility classes are not needed anymore, not to mention the classic problem of forgetting to use them.

``` java
public class Money {
   private  Currency currency;
   private  BigDecimal value;
   //behavior here
}
Product banana = new Product("banana", new Money(12, dollar));
Product pasta = new Product("pasta", new Money(4, dollar))
Money money = banana.getMoney().add(pineapple.getMoney());
```


With that it brought the motivation behind the kind of money creation. In addition to avoiding problems, for example, forgetting the money validation, mirrored and decapsulated code also guarantee higher quality code as sole responsibility money as an object and not just as data structure and bring the money to the domain of your application.