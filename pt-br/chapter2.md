# Mas ninguém passou por isso antes? Conhecimento da API


No capítulo anterior foi discutido as vantagens de se trazer o dinheiro como um tipo em vez de tratá-lo como apenas como tipo primitivo. Mas será que ninguém nunca passou por esse problema antes? A arte de “reinventar a roda” além de ser muito custosa, já que demanda muito tempo, passa a ser muito perigosa já que se tende a passar pelos mesmos problemas que um programador mais experiente já enfrentou. Uma famosa frase de *Clarice Lispecto*r diz: *"Quem caminha sozinho pode até chegar mais rápido, mas aquele que vai acompanhado, com certeza vai mais longe."*, ou seja, a melhor estratégia é se unir à um framework que já faz isso, assim é possível trocar experiências com programadores que já passaram por isso, não repetir os mesmos erros, contribuir com essa ferramenta, dessa forma, ela ficará cada mais e mais robusta e com menos erros.

Assim se pode unir a uma solução favorita para desenvolver o seu tipo dinheiro. Como lidar com esse tipo de problema é cada vez mais comum no mundo do desenvolvimento de software, o que acontece se cada um utilizar a sua solução específica? Por muitas razões se pode optar por uma solução, por exemplo, uma boa documentação, licença, razões comerciais, etc. Assim como integrar com cada uma delas? Caso se escolha uma é possível trocar para uma outra? Sabemos que ficar preso em um vendor não é algo bons para os negócios. Com esse objetivo nasce uma especificação Java para lidar com o tipo dinheiro a JSR 354. Ela tem como principal objetivo tratar dinheiro. 

Uma vez definindo a interface comum, cada empresa ou desenvolvedor poderá optar pela sua solução favorita ou trocá-la de forma transparente, para isso, basta que essa API implemente essa especificação. Esse comportamento é bem semelhante numa troca de banco de dados relacional no mundo Java, caso a aplicação siga a especificação e use as interfaces do JDBC, se pode trocar de banco tranquilamente, para isso, é necessário apenas trocar o driver, implementação, e tudo continuará funcionando desde que sua aplicação use apenas as interfaces.

Relembrando o conceito de dinheiro, de forma resumida, o dinheiro é composto por duas partes, a parte do valor que é a quantidade, assim representada de forma numérica, mas apenas com esse valor não conseguimos fazer muita coisa, precisamos da moeda. A moeda representa o “sistema do dinheiro” em comum uso, especialmente dentro de uma nação, seguindo essa definição o real, peso, dólar e euros são tipos de moedas.


Na money-api, será usado esse nome em vez de JSR 354, também será necessário representar tanto o valor numérico quanto a moeda. Para representar a moeda existe a interface CurrencyUnit ela precisa ser imutável e thread-safe.

