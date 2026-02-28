<script lang="ts">
	import type { Snippet } from "svelte";
	import type { HTMLButtonAttributes } from "svelte/elements";

	let isLeftHovered = $state(false);

	// Para extender os atributos de elementos do botão, podemos extender utilizando
	// HTMLButtonAttributes que é uma interface que contém todos os atributos de um elemento button, 
	// como disabled, type, onClick, etc.
	type Props = HTMLButtonAttributes & {
		// quando um prop possui uma argumento de callback, precisamos especificar o 
		// tipo do argumento do callback, como exemplo neste caso, a prop left é uma 
		// função que recebe um boolean e retorna um snippet, então o tipo da prop 
		// left seria Snippet<[boolean]>, ou seja, um snippet que recebe um array de boolean 
		// como argumento
		left?: Snippet<[boolean]>;
		right?: Snippet;
		children: Snippet<[boolean]>;
		size?: 'sm' | 'lg';
		shadow?: boolean;
	}

    // $props é uma função que retorna um objeto com as props passadas para o componente
    // como exemplo neste caso: <Button left={buttonLeft} />, a prop left é uma função 
    // que retorna um snippet
	// children é uma prop especial que contém o conteúdo entre as tags de abertura e 
	// fechamento do componente, como exemplo: <Button>Text</Button>, o children seria "Text"
    let { 
		left, 
		right, 
		children,
		size = 'sm', 
		shadow = false,
		...props // pegando todos os outros atributos de um elemento button (HTMLButtonAttributes) 
		// utilizando o rest operator
	}: Props = $props();
</script>

<!-- Podemos também usar um binding para facilitar o controle de qual classe utilizar utilizando "class:{className}"
Isso ajuda a evitar ter que escrever uma lógica para determinar qual classe utilizar, 
como exemplo: class={size === 'sm' ? 'sm' : size === 'lg' ? 'lg' : ''} -->
<!-- <button class:sm={size === 'sm'} class:lg={size === 'lg'} class:shadow> -->

<!-- Mas temos outra maneira melhor de fazer isso que é utilizando um objeto para passar as classes, 
onde a chave do objeto é o nome da classe e o valor é uma expressão booleana que determina se 
a classe deve ser aplicada ou não -->
<!-- E para múltiplas classes, podemos passar um array de classes, como: class={['sm', 'other-class-here']} -->
<button {...props} class={{
	['sm other-class-here']: size === 'sm',
	lg: size === 'lg',
	shadow
}}>
	{#if left}
		<div
			role="presentation"
			class="left-content" 
			onmouseenter={() => {
				isLeftHovered = true;
			}}
			onmouseleave={() => {
				isLeftHovered = false;
			}}
		>
			{@render left(isLeftHovered)}
		</div>
    {/if}

    {@render children(isLeftHovered)}

	{#if right}
		<div class="right-content">
			{@render right()}
		</div>
    {/if}
</button>

<style lang="scss">
    button {
		border: none;
		background-color: #ff3e00;
		color: #ffffff;
		padding: 0 20px;
		height: 45px;
		font-weight: bold;
		border-radius: 5px;
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;

		&:disabled {
			opacity: 0.6;
			cursor: not-allowed;
		}
		&:hover {
			background-image: linear-gradient(rgba(0, 0, 0, 0.4) 0 0);
		}
		&:active {
			background-image: linear-gradient(rgba(255, 255, 255, 0.1) 0 0);
		}
		&.sm {
			height: 45px;
		}
		&.lg {
			height: 55px;
			font-size: 20px;
		}
		&.shadow {
			box-shadow: 0 0 10px rgba(1, 1, 1, 0.3);
		}

		.left-content {
			margin-inline-end: 10px;
		}

		.right-content {
			margin-inline-start: 10px;
		}
	}
</style>
