<script lang="ts">
    let firstName = $state("");
    let lastName = $state("");

    let fullName = $derived.by(() =>{
       console.log("$derived", "Calculating full name");
        return `${firstName} ${lastName}`;
    });

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

<img {src} alt="A photo of {firstName}" />

<p>{bio}</p>
<p>{@html bio}</p>

<input bind:value={username} />
<input value={firstName} oninput={(e) => {
    firstName = e.currentTarget.value;
}} />
<input bind:value={lastName} />

<button onclick={() => {
    console.log(fullName);
}}>Get Full Name</button>

<style>
    h1 {
        color: red;
    }
</style>
