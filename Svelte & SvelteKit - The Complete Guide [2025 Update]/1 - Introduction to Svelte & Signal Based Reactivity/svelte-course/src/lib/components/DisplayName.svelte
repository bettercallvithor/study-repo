<script lang="ts">
    let firstName = $state("Vithor");
    let lastName = $state("Pedroso");
    let fullName = $derived.by(() => `${firstName} ${lastName}`);
    let username = $state("vithorpedroso");

    let src = 'https://picsum.photos/250/300';
    let bio = "Hello, <b>my name is Vithor</b> and I am a software developer.";

    $effect(() => {
        if (username || fullName) {
            console.log("User has a username or full name");
        }
    });
</script>

<h1>Hello {username || fullName}</h1>

<!-- Caso o nome da variável seja o mesmo nome do atributo HTML, podemos apenas definir em chaves
o nome da variável dentro da elemento HTML, como exemplo o src abaixo -->
<img {src} alt="A photo of {fullName}" />

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
<input bind:value={firstName} />
<input bind:value={lastName} />

<style>
    /* Todos estilos que adicionarmos aqui apenas afetará este componente
    Outros componentes que possuem h1 não serão afetados */
    h1 {
        color: red;
    }
</style>
