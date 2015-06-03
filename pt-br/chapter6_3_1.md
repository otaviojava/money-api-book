#### Predicate com moedas


Com o moneta é possível realizar predicate a partir da moeda, usando o inclusive e o exclusive. Ambos utilizam varargs, ou seja, é possível adicionar n elementos na condição:


* `isCurrency(CurrencyUnit... currencies)`: Retorna verdadeiro caso o MonetaryAmount tenha a uma das moedas especificadas.	
* `filterByExcludingCurrency(CurrencyUnit... currencies)`: Retorna verdadeiro caso o MonetaryAmount não tenha nenhuma das moedas especificadas.



[code]
public class PredicateMonetaryAmountCurrency {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");
        CurrencyUnit euro = Monetary.getCurrency("EUR");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        List<MonetaryAmount> justDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar)).collect(Collectors.toList());//[USD 10, USD 10, USD 8]

        boolean anyDollar = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allDollar = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//false

        List<MonetaryAmount> notDollar = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.filterByExcludingCurrency(dollar)).collect(Collectors.toList());//[BRL 10, BRL 9]

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.filterByExcludingCurrency(euro));//true

       
    }
}
[/code]

