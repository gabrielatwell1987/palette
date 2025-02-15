<script>
	import webp from '$lib/logos/pp.webp';
	import png from '$lib/logos/pp.png';

	let { title = 'PalettePro', subtitle = 'Discover your perfect palette' } = $props();
</script>

<section>
	<img src={webp} alt="PalettePro Logo" />

	<a href="/">
		<h1>{title}</h1>
	</a>

	<h3>{subtitle}</h3>
</section>

<style>
	section {
		text-align: center;
		background-color: #ccc;
		display: flex;
		gap: 2rem;
		justify-content: center;
		align-items: center;
		padding: 0.1rem;
		border-radius: 10px;

		& img {
			width: clamp(4em, 7vw, 15em);
			height: clamp(4em, 7vw, 15em);
		}

		& h1 {
			color: rgb(59, 59, 59);
			font-family: var(--sans-bold);
			font-size: clamp(1.25rem, 5vw, 4rem);
			font-weight: 800;
			letter-spacing: 2px;
			margin: 0;
			padding: 0;
		}

		& h3 {
			color: rgb(75, 75, 75);
			font-family: var(--sans);
			font-size: clamp(1.05rem, 1vw, 1.5rem);
			font-weight: 600;
			margin: 0;
			padding: 0;
		}
	}
</style>
