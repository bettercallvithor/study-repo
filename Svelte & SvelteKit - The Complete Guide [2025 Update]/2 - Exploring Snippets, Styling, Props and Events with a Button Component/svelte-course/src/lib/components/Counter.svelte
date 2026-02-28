<script lang="ts">
    let count = $state(0);
    let frequency = $state(1000);
    let paused = $state(false);

    $effect(() => {
        if (paused) {
            return;
        }

        const interval = setInterval(() => {
            console.log(count);
            count++;
        }, frequency);

        console.log("interval_id", interval);
        return () => clearInterval(interval);
    });
</script>

<h1>{count}</h1>

<button onclick={() => {
    count = 0;
    const _originalFrequency = frequency;
    frequency = 0;
    frequency = _originalFrequency;
}}>Reset</button>

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