Reactividade em aplicações UI é uma forma de atualizar a UI em favor de alterar algum dado da aplicação. Svelte usa signal based reactivity (Reação baseada em sinal).

* Sources (state): É um valor que pode ser lido e escrito. Não há nenhuma dependência e é derivados valores e reações (effects)
* Derived values: Apenas permite leitura e é um valor que é computado utilizando Pure Functions. Derived values dependem de outras sources. mas eles agem como a própria source.
* Side Effects (reactions): similar ao Derived Values, ele depende de outras sources. Mas ao invés de produzir um novo valor, ele produz um efeito colateral/side effect (como atualizar o DOM).