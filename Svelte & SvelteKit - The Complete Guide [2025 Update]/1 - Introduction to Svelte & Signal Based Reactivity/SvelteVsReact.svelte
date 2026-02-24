<script>
	// <script> é apenas executado 1 vez durante o load deste componente
	let count = $state(0);

	// caso fazemos sem o $derived aqui, como: let double = count * 2; - ele será o valor 0 para sempre
	// adicionando $derived, Svelte sabará que double depende de count e sempre que count for atualizado, double também será
	// atualizado conforme o valor de count seja alterado
	// Diferente de React, quando um estado é atualizado, a função do componente roda inteiramente novamente
	// então let double = count * 2; funcionaria corretamente - mas em Svelte, o <script> roda apenas 1 vez
	let double = $derived(count * 2);

	$effect(() => {
		console.log('from $effect', count, double);
	});
	
	function increment() {
		count++;
		console.log('from increment func', count, double);
	}
</script>

<button onclick={increment}>Count: {count}</button>
