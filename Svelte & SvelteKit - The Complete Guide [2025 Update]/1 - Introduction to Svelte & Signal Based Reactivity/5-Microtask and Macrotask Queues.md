Como vimos anteriormente, execuções `$effect` são postas na Microtask Queue - então é importante entender como Microtask queue funciona e em adicional, também Macrotask queue.

O código:
```js
console.log("1");

function log2() {
	console.log("2");
}

setTimeout(() => {
	console.log("3);
}, 0);

log2();

Promise.resolve().then(() => {
	console.log("4");
});

console.log("5");

queueMicrotask(() => {
	console.log("6");
});
```
Retorna o seguinte valor: 125463 - Por que?

1. **Modelo de Execução do JavaScript**
	1. O JavaScript é uma linguagem de thread única que utiliza uma pilha de chamadas (call stack) para gerenciar a execução do código. Quando uma função é chamada, ela é adicionada à pilha, executada e depois removida.
2. **Macrotasks (tarefas macro)**
	1. Estas incluem funções que são agendadas com `setTimeout`. Elas são colocadas na macrotask queue. Quando a pilha de chamadas está vazia, o event loop processa uma tarefa da macrotask queue.
3. **Microtasks (tarefas micro)**
	1. Estas são tarefas que, por exemplo, vêm de promessas (Promises). Elas são colocadas na microtask queue e têm prioridade em relação às macrotasks. Isso significa que o event loop sempre processa a microtask queue antes de passar para a próxima macrotask.

Agora, analisando o código que resulta na saída 125463:
- Primeiro, o número 1 é exibido.
- Depois, a função que imprime 2 é chamada e executada.
- Em seguida, a execução continua com o console.log 5.
- O console.log 4 é agendado através da resolução de uma promessa e colocado na microtask queue.
- O console.log 6 também é adicionado à microtask queue.
- Assim que o call stack está vazio, a microtask queue é processada, imprimindo 4 e 6 antes de voltar para as macrotasks e imprimir 3.
	- Microstasks tem prioridade de execução antes das Macrotasks

Portanto, a ordem em que os números são mostrados é 1 (de console.log), 2 (função chamada), 5 (console.log), 4 (promessa resolvida na microtask queue), 6 (outra microtask) e finalmente, 3 (da macrotask queue).