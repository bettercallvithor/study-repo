<script lang="ts">
	import { untrack } from "svelte";

    let randomNumber = $state(Math.floor(Math.random() * 10));
    let doubleRandomNumber = $derived(randomNumber * 2);

    // estamos utilizando untrack() aqui para evitar que um estado seja colocado dentro de outro estado
    // assim estamos avisando para o Svelte que não queremos que o estado history observe o valor de randomNumber e sim
    // apenas o valor inicial dele
    let history: number[] = $state([untrack(() => randomNumber)]);

    // let doubleRandomNumber = $state();

    // Não podemos atualizar o valor de um estado diretamente utilizando a Runa $effect - como sabemos, as funções $effect() 
    // são executadas dentro de um microtask, no qual são enfileiradas para executar sempre após a execução do código síncrono.
    // Então, ao clicar no botão "Generate" abaixo, perceba que irá printar o valor de doubleRandomNumber anterior, pois ainda não
    // foi atualizado, e a função $effect() ainda não foi executada.
    // $effect(() => {
    //     doubleRandomNumber = randomNumber * 2;
    // });

    // $effect(() => {
    //     // Isso causará um loop infinito, pois toda vez que o valor de randomNumber for atualizado, a função $effect() será executada, 
    //     // e o valor de doubleRandomNumber também será atualizado, o que fará com que a função $effect() seja executada novamente.
    //     // Isso pois este $effect está observando o valor de randomNumber e history
    //     history.push(randomNumber);
    // });

    // $effect(() => {
    //     // A função untrack() é utilizada para evitar que a função $effect() observe os valores que estão sendo utilizados dentro dela
    //     // ou seja, a função $effect() não irá observar o valor de randomNumber e history, e não irá causar um loop infinito neste caso;
    //     // mas isso não irá atualizar history ou randomNumber, pois a função $effect() não irá observar os valores que estão sendo utilizados dentro dela
    //     // e isso é um problema caso queremos atualizar o valor de history ou randomNumber dentro da função $effect(), 
    //     // pois a função $effect() não irá observar os valores que estão sendo utilizados dentro dela
    //     untrack(() => history.push(randomNumber));
    // });

    // $effect(() => {
    //     // Para isso, iremos colocar o randomNumber fora do untrach(), no qual faz o $effect() observar o valor de randomNumber, 
    //     // e dentro do untrack() irá atualizar o valor de history, sem observar o valor de history, evitando assim o loop infinito.
    //     randomNumber;

    //     // mas isso é um problema, caso o randomNumber já recebeu o mesmo número que anteriormente (1 execução antes), $effect não será executado novamente
    //     // pois já executou com o mesmo valor anteriormente 1 passo atrás e o valor de history não será atualizado
    //     untrack(() => {
    //         history.push(randomNumber);
    //     });
    // });
    // mas perceba que não precisamos utilizar $effect neste caso, por que não utilizar ele dentro
    // do onclick do botão abaixo? Isso é uma segurança e sabemos sempre que ele será colocado
    // dentro da lista sempre que o botão for clicado
</script>

<h2>The random number is: {randomNumber}</h2>
<h2>Double random number is: {doubleRandomNumber}</h2>
<p>History: {history}</p>

<button onclick={() => {
    randomNumber = Math.floor(Math.random() * 10);
    console.log({randomNumber, doubleRandomNumber});
    history.push(randomNumber);
}}>Generate</button>
