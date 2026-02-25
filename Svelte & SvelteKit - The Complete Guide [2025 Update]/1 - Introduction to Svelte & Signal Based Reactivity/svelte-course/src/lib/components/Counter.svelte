<script lang="ts">
    let count = $state(0);
    let frequency = $state(1000);
    let paused = $state(false);

    $effect(() => {
        if (paused) {
            return;
        }

        const interval = setInterval(() => {
            // Mesmo com essa dependência do count aqui, sempre que o count for alterado,
            // o $effect não será reexecutado, pois o count é atualizado dentro do setInterval
            // que roda de forma assíncrona, e o $effect só é reexecutado quando as dependências 
            // são alteradas de forma síncrona.
            console.log(count);
            count++;
        }, frequency);

        console.log("interval_id", interval);

        // Sempre que o frequency for alterado, o efeito será reexecutado, 
        // limpando o intervalo anterior e criando um novo com a nova frequência.
        // Caso isso não seja feito, múltiplos intervalos podem ser criados, 
        // causando um comportamento inesperado e sobrecarregando a execução do código
        // com vários intervals.
        return () => clearInterval(interval);
    });
</script>

<h1>{count}</h1>

<button onclick={() => {
    count = 0;

    // Força a reexecução do $effect para reiniciar o intervalo com o count zerado,
    // mas sem alterar a frequência, para isso, alteramos o valor da frequência para um valor diferente e depois voltamos para o valor original, isso faz com que o $effect seja reexecutado, limpando o intervalo anterior e criando um novo com a frequência original e o count zerado.
    const _originalFrequency = frequency;
    frequency = 0;
    frequency = _originalFrequency;
}}>Reset</button>

<!-- O Pause funciona aqui pois, $effect tem uma dependência em pause, então sempre que o valor de pause
for alterado, o $effect será executado e antes de executar ele executará a função de limpeza no qual
é: `return () => clearInterval(interval);` que limpa o interval parando a incrementação do count -->
<button onclick={() => {
    paused = !paused;
}}>{paused ? "Play" : "Pause"}</button>

<button onclick={() => {
    frequency *= 2;
}}>Slower</button>

<button onclick={() => {
    frequency /= 2;
}}>Faster</button>

<p>{frequency} ms</p>