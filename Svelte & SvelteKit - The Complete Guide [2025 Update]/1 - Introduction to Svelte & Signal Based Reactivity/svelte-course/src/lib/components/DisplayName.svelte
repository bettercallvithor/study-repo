<script lang="ts">
    let firstName = $state("");
    let lastName = $state("");

    // Podemos também utilizar $derived(`$firstName ${lastName}`) para simplificar a sintaxe, mas para exemplificar o uso do $derived, vamos usar a função by
    // que é utilizada para criar verificações mais complexas
    // $derived é uma função que é read-only e é utilizada para criar valores derivados de outros valores reativos, ou seja, 
    // ele é utilizado para criar valores que dependem de outros valores reativos, como por exemplo, o fullName que depende do firstName e lastName
    let fullName = $derived.by(() =>{
        // isso que estamso fazendo com o console.log é um side-effect que estamos colocando dentro de uma
        // pure function (no qual está errado) - mas apenas estou utilizando aqui para efeitos de demonstração
        console.log("$derived", "Calculating full name");

        // Svelte faz memoization do valor retornado por $derived, ou seja, ele só irá recalcular o valor de fullName quando firstName ou lastName mudar, 
        // caso os mesmos valores já foram computados, ele não irá executar essa função novamente, mas sim retornar o valor memoizado
        // então não espere que o console.log seja executado toda vez que o componente for renderizado, ele só será executado quando firstName ou lastName mudar
        return `${firstName} ${lastName}`;
    });
    // ao invés de derived, também podemos fazer uma função como: const fullName = () => `${firstName} ${lastName}` - 
    // mas isso não é recomendado, pois toda vez que o componente for renderizado, essa função será executada, 
    // mesmo que firstName e lastName não tenham mudado.
    // caso fullName() esteja sendo utilizado 20x no elemento HTML, essa função será executada 20x toda vez que o componente for renderizado, 
    // mesmo que firstName e lastName não tenham mudado. Isso porque ele não faz memoization como o $derived, então ele irá executar a função 
    // toda vez que o componente for renderizado, o que pode causar problemas de performance em casos mais complexos.

    let username = $state("");

    let src = 'https://picsum.photos/250/300';
    let bio = "Hello, <b>my name is Vithor</b> and I am a software developer.";

    $effect(() => {
        if (username || fullName) {
            console.log("User has a username or full name");
        }
    });
</script>

<h1>{username || fullName}</h1>

<!-- Caso o nome da variável seja o mesmo nome do atributo HTML, podemos apenas definir em chaves
o nome da variável dentro da elemento HTML, como exemplo o src abaixo -->
<img {src} alt="A photo of {firstName}" />

<!-- Os elementos HTML não interpretam tags HTML dentro de strings por questões de segurança -->
<p>{bio}</p>
<!-- Para isso devemos usar a diretiva {@html} para que o Svelte 
interprete as tags HTML dentro da string, como exemplo abaixo: -->
<!-- Mas para usar isso, temos que ter certeza que a string é segura, ou seja, não contém código malicioso, 
pois isso pode abrir brechas de segurança em nossa aplicação. -->
<p>{@html bio}</p>

<!-- Em Svelte, podemos fazer o Input que altera um valor da seguinte forma: -->
<!-- <input 
    value={username} 
    oninput={(e) => {
        username = e.currentTarget.value;
    }} 
/> -->
<!-- Ou para simplificar, podemos utilizar a palavra chave `bind` que faz este trabalho de oninput
para nós: -->
<input bind:value={username} />
<input value={firstName} oninput={(e) => {
    firstName = e.currentTarget.value;
}} />
<input bind:value={lastName} />

<button onclick={() => {
    // Perceba que o fullName é um $derived que executa um console.log toda vez que é recalculado
    // mas aqui ele não executa o console.log no fullName, pois ele já foi memoizado, ou seja, 
    // ele já tem o valor calculado e não precisa recalcular e apenas retorna o valor memoizado, então o console.log dentro do $derived não é executado
    console.log(fullName);
}}>Get Full Name</button>

<style>
    /* Todos estilos que adicionarmos aqui apenas afetará este componente
    Outros componentes que possuem h1 não serão afetados */
    h1 {
        color: red;
    }
</style>
