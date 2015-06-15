# One specification to money, is it really worth it?

Money is the main way to do exchange goods, products, buy services etc. It is absolutely represented in several Java programs. But after all, does make sense a money type? Why not just use types that are already on Java such `Double`, `Float`, `String`, `BigDecimal`, etc.? Are there solutions better than it? What does happen with the encapsulation if the choice is a type that already exist on Java? the utilitarian is really useful, but if a developer need to remember to do something, they may forget and the result will a disaster. Once was talked about the money question is born the first question: Did nobody live this problem? Is it worth reinventing the wheel? To solve these problems the money specification was born. 
	
The **JSR 354**, is a specification whose the goal is take care of the money, solve some general problems and make the life of the Java developer easier. From this specification will be more obvious work with money on standardization way. To understand better the API, this cook book will be divided em some parts:

In the first chapter will discuss the motivation behind has a money type on a system, the benefits on design, it hasn't messy code, the easy maintenance and more code object-oriented.

Creation of a money type, but did nobody live this problem before? This chapter will answer this question, talking the motivation to has a specification and the introduction to the API.

Extract values and do some operations from monetary amount, it is the goal of operations with query and operator: Will show how the MonetaryOperator and MonetaryQuery classes work, the difference between them, and the MonetaryOperators and MonetaryQueries utilitarian classes and how create the own query and operation with money.


The Formatting a monetary amount and retrieve it from String beyond the create custom a formatter. It is the objective of this chapter.	

Yes, there are support to Java 8! Know the functions that already exist on reference implementation to work with Stream on monetary amount.

Exchange rate, sum money with different currencies will return an exception. In this point will show how do exchange rate of a monetary amount and find rate from a specific date.


Working with Java EE such JSF converter, CDI, JPA, etc. it will show in this chapter and how work the money API with these technologies.

I hope that the reader enjoy this project that was made to Java community to Java community.


