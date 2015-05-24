# Introdução

Boa parte dos sistemas de informação que desenvolvemos manipulam valores monetários e moedas. Entretanto, é no mínimo curioso perceber que os conceitos de "Dinheiro" e "Moeda" comumente são relegados a tipos primitivos como Float, Double e BigDecimal. Sistemas melhor encapsulados acabam implementando cada um suas próprias classes de "Dinheiro" e "Moeda". Mas até quando os desenvolvedores Java teriam que continuar resolvendo os mesmos problemas sem ter uma solução de referência padronizada?

A JSR 354 (Money and Currency API) é um esforço para definir uma API e fornecer uma implementação de referência para resolver os problemas definidos em torno dos conceitos de "Dinheiro" e "Moeda".