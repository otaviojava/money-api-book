# One specification for money, is it really worth it?

Money is a verifiable record that is accepted as payment for goods and services. Java developers deal with money in their day-to-day jobs. But do we really need to introduce a specification for a money API? Why don't we just use the types that are already available in the JDK such as `Double`, `Float`, `String`, `BigDecimal`?  What are the alternatives? The Money-API specification was introduced to standardise the way we approach this problem.

**JSR 354**  is a specification for a general purpose Money-API that aims to make the life of the Java developer easier. To give clearer picture of the API, this cook book will be divided into the following parts:

In the first chapter we will go through the motivation behind the Money-API, the benefits of its design, its lack of code-clutter,  ease-of-maintenance and object-oriented design.

Extracting and applying operations on monetary values. Working with query and operator: Here we will see how the `MonetaryOperator` and `MonetaryQuery` classes work, the difference between them.  `MonetaryOperators` and `MonetaryQueries` are utility classes used to operate and query money respectively.

The Formatting Money chapter shows how can we parse or convert a `String` into a monetary value without introducing a custom formatter.

The API supports Java 8, so we will see how we can work with `Streams` of monetary values.

Working with **Java EE** such as **JSF** converters, **CDI**, **JPA**, **bean validator** etc., will be the subject of this chapter and how the API works with these technologies.


We hope that the reader we will enjoy reading this book as well as using the API that was developed for the Java community by the Java community.
