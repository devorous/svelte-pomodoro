<script lang="ts">
    let { start, pause, isPaused } = $props();
</script>
<div>
    {#if isPaused}
        <button onclick= {pause}>Pause</button>
    {:else}
        <button onclick={start}>Start</button>
    {/if}
</div>

<style>
    button{
        user-select: none;
        background-color: #333;
        border: 1px solid black;
        border-radius: 5px;
        font-size: clamp(1rem,10vmin,3rem);
        width: clamp(4rem,35vmin,12rem);
        padding: 3vmin;
        margin-left: 5vmin;
        margin-right: 5vmin;
    }
    button:hover{
        background-color: #444;
        cursor: pointer;
    }
</style>