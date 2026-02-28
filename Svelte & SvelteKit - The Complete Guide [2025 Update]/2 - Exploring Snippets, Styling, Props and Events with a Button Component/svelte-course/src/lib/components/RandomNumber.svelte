<script lang="ts">
	import { onDestroy, onMount, tick, untrack } from "svelte";

    let randomNumber = $state(Math.floor(Math.random() * 10));
    let doubleRandomNumber = $derived(randomNumber * 2);
    let history: number[] = $state([untrack(() => randomNumber)]);
    let historyPTag: HTMLParagraphElement;

    onMount(() => {
        console.log("onMount");
        return () => {
            console.log("onDestroy");
        }
    });

    onDestroy(() => {
        console.log("onDestroy");
    });

    $effect(() => {
        history.length;
        console.log("$effect", historyPTag?.innerText);
        return () => {
            console.log("$effect cleanup");
        }
    });

    $effect.pre(() => {
        history.length;
        console.log("$effect.pre", historyPTag?.innerText);

        tick().then(() => {
            console.log("$effect.pre - After tick", historyPTag?.innerText);
        });

        return () => {
            console.log("$effect.pre cleanup");
        }
    });
</script>

<h2>The random number is: {randomNumber}</h2>
<h2>Double random number is: {doubleRandomNumber}</h2>

<p bind:this={historyPTag}>History: {history}</p>

<button onclick={() => {
    randomNumber = Math.floor(Math.random() * 10);
    console.log({randomNumber, doubleRandomNumber});
    history.push(randomNumber);
}}>Generate</button>
