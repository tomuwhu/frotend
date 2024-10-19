<script lang="ts">
    import { onMount } from 'svelte'
    var data = []
    var newdata = {title: '', views: 0}
    onMount(async () =>  { 
        data = await fetch('http://localhost:3000/posts')
            .then(r => r.json())
    })
</script>
{#each data as item}
    <h4>{item.title} {item.views}</h4>
{/each}
<input type="text" bind:value={newdata.title}>
<input type="number" bind:value={newdata.views}>
<button on:click={async () => {
    let answ = await fetch('http://localhost:3000/posts', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(newdata)
    })
    if (answ.ok) {
        data = await fetch('http://localhost:3000/posts')
            .then(r => r.json())
        newdata = {title: '', views: 0}
    }
}}>Felvesz újként</button>
<style>
    h4 {
        padding: 0px;
        margin: 0px;
    }
</style>