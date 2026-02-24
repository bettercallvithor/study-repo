<script>
	// $state é built-in no Svelte - não precisamos importar ele de lugar nenhum
	// o compilador já sabe o que fazer com isso
	let count = $state(0); // essas funções que começam com $ são chamadas de Runes
	let double = $derived(count * 2);
	// $state e $derived são um das Runes mais utilizadas

	// não precisamos declarar quais que são as dependências do $effect, o compilador Svelt fará essa verificação
	// na hora de compilar o código, caso apenas o count esteja sendo utilizado dentro do $effect, apenas este $effect
	// será chamado caso o count mudar
	// $effect é utilizado para quando um estado de algo mudar e precisamos executar algum efeito
	// Apenas utilize $effect para realizar um side-effect, como exemplo: atualizar o DOM
	$effect(() => {
		console.log('from $effect', count, double);
	});
	
	function increment() {
		// não precisamos utilizar algum tipo de função para atualizar o estado, como exemplo em react o useState / setCount
		// svelte é um compilador e deixa você com a liberdade de utilizar essa sintaxe que o torna mais simples
		count++;

		// Podemos ver no console que o estado é atualizado de forma síncrona e imediata, ao contrário do React em que é feita de forma
		// assíncrona
		// Como exemplo, supondo que count = 0:
		// setCount(count + 1)
		// console.log(count) ==> irá imprimir 0, pois count ainda não foi atualizado de forma assincrona
		// double que é um $derived também é atualizado de forma síncrona e imediata
		console.log('from increment func', count, double);
	}

	// $derived => é rodado de forma síncrona
	// $effect => é colocado na fila de microtarefas para ser executado
</script>

<!-- 
por de baixo dos panos, Svelte possui um efeito colateral sempre que count for alterado, para alterar apenas este espaço de código
caso `count` seja alterado, Svelte apenas irá alterar o botão e nada mais na página
-->
<button onclick={increment}>Count: {count}</button>
