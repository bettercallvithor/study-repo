# Pure Functions

Uma **Pure Function** (ou **função pura**) é uma função que não possui nenhum _side effect_ (efeito colateral) durante sua execução. Por exemplo, uma função `sum(num1, num2)` deve sempre retornar a soma desses dois números. No entanto, se ela alterar algo no DOM, deixará de ser considerada uma Pure Function.

Uma Pure Function é, basicamente, uma função que recebe uma entrada e produz uma saída, sem realizar qualquer operação fora do seu próprio escopo.

Uma Pure Function pode utilizar _memoization_, desde que isso não altere seu comportamento previsível nem gere efeitos colaterais externos.
## Memoization

Memoization armazena em cache os resultados de uma função, evitando que ela seja executada novamente quando receber as mesmas entradas e, consequentemente, retornaria o mesmo output. Por exemplo, se `sum(num1, num2)` receber `num1 = 2` e `num2 = 2` e já tiver sido executada anteriormente retornando o valor 4, ao receber 2 e 2 novamente, sabemos que essa combinação já foi utilizada e que o resultado será 4.

Com uma Pure Function, sabemos que não haverá nenhum side effect além do cálculo em si e podemos fazer o cache seguramente. Isso torna a memoization especialmente útil em funções puras muito pesadas, que exigem bastante processamento, quando é necessário evitar recalcular o mesmo resultado repetidamente.