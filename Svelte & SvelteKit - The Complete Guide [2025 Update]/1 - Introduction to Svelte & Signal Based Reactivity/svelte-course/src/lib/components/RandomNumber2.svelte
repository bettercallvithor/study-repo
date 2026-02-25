<script lang="ts">
	import { onDestroy, onMount, tick, untrack } from "svelte";

    let randomNumber = $state(Math.floor(Math.random() * 10));
    let doubleRandomNumber = $derived(randomNumber * 2);
    let history: number[] = $state([untrack(() => randomNumber)]);
    let historyPTag: HTMLParagraphElement;

    // Essa função sempre será executada quando o componente for montado na tela, ou seja, 
    // quando ele for renderizado pela primeira vez.
    // Apenas executado 1x
    onMount(() => {
        console.log("onMount");
        return () => {
            console.log("onDestroy");
        }
    });
    // ou
    // Essa função sempre será executada quando o componente for destruído, ou seja, 
    // quando ele for removido da tela.
    // Apenas executado 1x
    onDestroy(() => {
        console.log("onDestroy");
    });

    // $effect é sempre carregado após o load do DOM, então historyPTag já estará disponível 
    // para ser utilizado dentro do $effect() e não irá causar um erro de variável nula
    // perceba acima que historyPTag não está inicializado
    $effect(() => {
        // Agora, $effect vai observar history.length e agora é reativo,
        // então toda vez que history.length for alterado, a função $effect() será executada
        history.length;
        console.log("$effect", historyPTag?.innerText);

        // Este retorno é uma função de limpeza (cleanup function) que é chamada antes da próxima 
        // execução do $effect() ou quando o componente é destruído.
        // então sempre que o $effect() for executado novamente, a função de limpeza será 
        // chamada antes da execução do próximo $effect(),
        return () => {
            console.log("$effect cleanup");
        }
    });

    // $effect.pre é chamado antes da renderização do DOM na UI, então historyPTag ainda não estará disponível para ser utilizado 
    // dentro do $effect.pre() e irá causar um erro de variável nula
    // (neste caso estará undefined)
    // caso seja atualizado o valor de history.length, o historyPTag já estará disponível,
    // mas com o valor anterior, pois o DOM ainda não foi atualizado
    // .pre() é executado antes de tudo, até onMount()
    $effect.pre(() => {
        history.length;
        console.log("$effect.pre", historyPTag?.innerText);

        // A função tick() no Svelte é utilizada para garantir que todas as mudanças de estado pendentes sejam 
        // aplicadas ao DOM antes de continuar com a execução do código. Quando você chama tick(), ela retorna 
        // uma promessa que resolve assim que as atualizações de estado são refletidas no DOM.
        tick().then(() => {
            console.log("$effect.pre - After tick", historyPTag?.innerText);
        });

        // Este retorno é uma função de limpeza (cleanup function) que é chamada antes da próxima 
        // execução do $effect() ou quando o componente é destruído.
        // então sempre que o $effect() for executado novamente, a função de limpeza será 
        // chamada antes da execução do próximo $effect(),
        return () => {
            console.log("$effect.pre cleanup");
        }
    });
</script>

<h2>The random number is: {randomNumber}</h2>
<h2>Double random number is: {doubleRandomNumber}</h2>

<!-- bind:this é uma diretiva do Svelte que permite vincular um elemento DOM a uma variável do nosso componente.
então historyPTag é uma variável do nosso componente que está vinculada ao elemento <p> que exibe o histórico dos números aleatórios gerados. -->
<p bind:this={historyPTag}>History: {history}</p>

<button onclick={() => {
    randomNumber = Math.floor(Math.random() * 10);
    console.log({randomNumber, doubleRandomNumber});
    history.push(randomNumber);
}}>Generate</button>
