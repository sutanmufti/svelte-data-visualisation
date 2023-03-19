<script lang='ts'>
    import { onMount } from "svelte";
    let buttonNames = [0,1,2,3]
    let buttons: HTMLButtonElement[] = []

    let initiated = false;
    onMount(()=>{
        if (!initiated){
            buttons.forEach(_=>{
                _.click()
            })
        }
        initiated = true
    })
</script>
{#each buttonNames as d,i}
<div>
    <button on:click={()=>{
        alert("hello from button! this is self clicked in onMount call back. binding is awesome! button number: " + i);
        buttons[i].innerHTML = "Button " + i + " was clicked"
        }} bind:this={buttons[i]}>button {i}</button>
</div>
{/each}