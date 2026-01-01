<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<main>
	{@render children()}
</main>



<style>
	main{
		display:flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
		transform: translateY(-10vh);
	}
</style>