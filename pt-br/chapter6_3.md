### Predicates


Operação de predicate é quando dado uma entrada o retorno é um valor booleano, ou seja, verdadeiro ou falso. Dentro do Stream o predicate pode ser utilizado como filtragem, filtrar por uma moeda específica, ou para match, verificar se existe algum elemento da lista ou todos batem com a condição.


[code]

public class BooleanMonetaryAmount {

    public static void main(String[] args) {
        CurrencyUnit dollar = Monetary.getCurrency("USD");
        CurrencyUnit real = Monetary.getCurrency("BRL");


        MonetaryAmount money = Money.of(10, dollar);
        MonetaryAmount money2 = Money.of(10, real);
        MonetaryAmount money3 = Money.of(10, dollar);
        MonetaryAmount money4 = Money.of(9, real);
        MonetaryAmount money5 = Money.of(8, dollar);

        Stream<MonetaryAmount> justDollars = Stream.of(money, money2, money3, money4, money5)
                .filter(MonetaryFunctions.isCurrency(dollar));

        boolean anyMatch = Stream.of(money, money2, money3, money4, money5)
                .anyMatch(MonetaryFunctions.isCurrency(dollar));//true

        boolean allMatch = Stream.of(money, money2, money3, money4, money5)
                .allMatch(MonetaryFunctions.isCurrency(dollar));//true
    }
}

[/code]
