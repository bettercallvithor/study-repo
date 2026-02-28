<script lang="ts">

</script>

<!-- Snippets nos permitem reutilizar o conteúdo em um componente sem precisar
extrair esse conteúdo em um arquivo ou criar um novo componente -->
{#snippet sum(a: number, b: number)}
    {#snippet bold(x: number)}
        <b>{x}</b>
    {/snippet}

    <!-- Este snippet apenas vai funcionar aqui, pois ele está dentro do escopo do snippet sum -->
    <span>{a} + {b} = {@render bold(a+b)}</span>
{/snippet}

<button>Text</button>

<!-- Para renderizar um snippet, devemos usar a diretiva @render para renderizar um snippet
presente neste componente -->
{@render sum(2, 2)}

<!-- Isso não vai funcionar, pois este snippet está dentro do snippet de sum -->
<!-- {@render bold(123)} -->

<style>
    button {
		border: none;
		background-color: #ff3e00;
		color: #ffffff;
		padding: 0 20px;
		height: 45px;
		font-weight: bold;
		border-radius: 5px;
		cursor: pointer;
	}
</style>
