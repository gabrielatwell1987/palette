<script>
	import '../app.css';
	import Github from '$lib/components/Github.svelte';
	import Nav from '$lib/components/Nav.svelte';

	let { children } = $props();
</script>

<main>
	<Nav />

	{@render children()}

	<Github />
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		gap: 2rem;
		min-height: 100vh;
		padding: 1rem;
		background-color: #f0f0f0;
	}
</style>
