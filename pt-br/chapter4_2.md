## A classe MonetaryAmountFormatSymbols

Existe também a interface ```MonetaryAmountFormatSymbols```, que de forma semelhante a classe ```DecimalFormat``` com a classe ```Number```, tem o objetivo de realizar formatações do dinheiro a partir de configurações de símbolos, moedas, quantidade mínima e máxima de dígitos antes e depois da vírgula, etc.


[code]
public class MonetaryAmountFormatSymbolsExample {

    public static void main(String[] args) {
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        MonetaryAmount money = Money.of(12, currency);
        MonetaryAmountFormat defafult = MonetaryAmountFormatSymbols.getDefafult();
        String format = defafult.format(money);//R$ 12,00
        MonetaryAmount moneyParsed = Money.parse(format, defafult);//or using defafult.parse(format);

    }
}
[/code]


Caso seja necessário configurar as informações como quantidade mínima, moeda, etc. Existem duas classes: A primeira é a MonetaryAmountSymbols com ela é possível definir os símbolos que serão utilizados, por exemplo, símbolo da moeda, separador, etc. A classe MonetaryAmountNumericInformation cuidará das informações com relação a formatação do valor numérico, por exemplo, o número mínimo e máximo de dígitos antes e depois da vírgula. 

[code]
public class MonetaryAmountFormatSymbolsExample2 {

    public static void main(String[] args) {
        MonetaryAmountFormatSymbols defafult = MonetaryAmountFormatSymbols.getDefafult();
        MonetaryAmountSymbols amountSymbols = defafult.getAmountSymbols();
        MonetaryAmountNumericInformation numericInformation = defafult.getNumericInformation();
        
    }
}
[/code]



Também é possível definir qual implementação que será utilizada na serialização do objeto. Para isso existe a classe funcional MonetaryAmountProducer, com ela é possível definir sua própria implementação a partir do number e da moeda. O moneta por padrão já vem com três implementações:


FastMoneyProducer produtor de MonetaryAmount com a implementação FastMoney.
MoneyProducer produtor de MonetaryAmount com a implementação Money.
RoundedMoneyProducer produtor de MonetaryAmount com a implementação RoundedMoney, nela é possível passar um MonetaryOperator que será utilizado na construção dessa implementação caso não seja informado um operador de arredondamento será utilizado o MonetaryOperators.rounding(). Lembrando que MonetaryOperator dentro da implementação RoundedMoney será utilizado como agente arredondador, ou seja, para cada operação esse operador será chamado.



[code]
public class MonetaryAmountFormatSymbolsExample3 {

    public static void main(String[] args) {
        MonetaryAmountSymbols symbols = new MonetaryAmountSymbols(Locale.US);// new MonetaryAmountSymbols();
        symbols.setCurrencySymbol("Mon");
        MonetaryAmountFormat formater = MonetaryAmountFormatSymbols.of(symbols, new MoneyProducer());
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        String text = formater.format(Money.of(10, currency));//Mon 10.00

    }
}
[/code]

Também é possível passar uma String como pattern para a formatação, esse String segue o mesmo padrão da classe DecimalFormat.

[code]
public class MonetaryAmountFormatSymbolsExample3 {

    public static void main(String[] args) {
        MonetaryAmountSymbols symbols = new MonetaryAmountSymbols(Locale.US);// new MonetaryAmountSymbols();
        symbols.setCurrencySymbol("Mon");
        MonetaryAmountFormat formater = MonetaryAmountFormatSymbols.of("¤ ###,###.00", symbols, new MoneyProducer());
        CurrencyUnit currency = Monetary.getCurrency("BRL");
        String text = formater.format(Money.of(10_000_00, currency));//Mon 1,000,000.00
        System.out.println(text);
    }
}
[/code]