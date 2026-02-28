<script lang="ts">
	import Button from "$lib/components/Button.svelte";
	import { Search, AlarmClock } from "lucide-svelte";
</script>


<!-- <DisplayName /> -->
<!-- <RandomNumber /> -->
<!-- <Counter /> -->

<!-- 
{#snippet left()}
	<Search />
{/snippet}
<Button {left}>Text</Button> -->
<Button size="sm" shadow disabled>
	<!-- Mesmo passando o snippet aqui dentro, ele entende como se fosse uma prop e passa 
	o snippet para a prop left, pois o nome do snippet é o mesmo da prop, ou seja, left -->
	<!-- left possui um argumento de callback, ou seja, é uma função que recebe um argumento e
	em que o componente que está utilizando (no caso o Button) é responsável por passar o 
	argumento para a função -->
	{#snippet left(isHovered: boolean)}
		{#if isHovered}
			<Search />
		{:else}
			<AlarmClock />
		{/if}
	{/snippet}

	<!-- children também pode receber um argumento de callback -->
	<!-- ou caso não precise de nenhum argumento, apenas utilizar o valor normalmente (neste caso
	apenas "Text") -->
	{#snippet children(isLeftHovered)}
		Text {isLeftHovered ? "Hovered" : "Not Hovered"}
	{/snippet}
	{#snippet right()}
		<AlarmClock />
	{/snippet}
</Button>
