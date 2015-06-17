# One specification for money, is it really worth it?

Money is the main way to exchange goods, purchase products and services etc. It is used in several Java programs. But after all, does it make sense to define a money type? I mean why not just use types that are already in Java such as `Double`, `Float`, `String`, `BigDecimal`, etc.? Aren't there any better solutions? What happens to encapsulation if the choice is a type that already exists? The utility is really useful, but forces the developer to remember to do something, but, human-memory should not be heavily relied upon. Once we speak about the Money concerns, then and there is born the first question: Did no one ever encounter this problem? Is it worth reinventing the wheel? To solve these problems the Money specification was born.

The **JSR 354**, is a specification whose the goal is to take care of Money, solve some general problems and make the life of the Java developer easier. From this specification there will be more obvious work with money in a standardised way. To understand the API better, this cook book will be divided into some parts:

In the first chapter we will discuss the motivation behind defining a Money type in the system, the benefits of its design, its lack of code-clutter,  ease-of-maintenance and object-oriented design.

Creating a money type - but did anybody have this problem before? This chapter will answer this question, talking about the motivation to create a specification and the introduction to the API.

Extracting values and doing some operations from a monetary amount, is the style of operations with query and operator: Here we will see how the `MonetaryOperator` and `MonetaryQuery` classes work, the differences between them, and the `MonetaryOperators` and `MonetaryQueries` utility classes and how to create a query and operation on money.

The Formatting of a monetary amount and retrieving it from String beyond the creation of a custom formatter. It is the objective of this chapter.

Yes! there are support to Java 8. Know the functions that exist on the reference implementation to work with **Stream**'s on monetary amounts.

Exchange rate and sum  money with different currencies will return an exception. At this point we will show you how to do an exchange rate of a monetary amount and find a rate from a specific date.

Working with **Java EE** such as **JSF** converters, **CDI**, **JPA**, **bean validator** etc., will be the subject of this chapter and how does the money API work with these technologies.

I hope that the reader has fun with this project that was made for the Java community by the Java community.