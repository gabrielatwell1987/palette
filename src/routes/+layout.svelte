<script>
	import '../app.css';
	import Github from '$lib/components/Github.svelte';

	let { children } = $props();
</script>

<main>
	{@render children()}

	<Github />
</main>

<style>
</style>
