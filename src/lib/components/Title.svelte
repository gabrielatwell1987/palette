<script>
	let { title = 'PixiPalette' } = $props();
</script>

<section>
	<img src="/logos/pixipalette.webp" alt="PalettePro Logo" />

	<a href="/">
		<h1>{title}</h1>
	</a>
</section>

<style>
	section {
		text-align: center;
		background-color: transparent;
		display: flex;
		gap: 1.5rem;
		justify-content: center;
		align-items: center;
		padding: 0.1rem;
		border-radius: 10px;
		margin: 0;
		padding: 0;

		& img {
			width: clamp(6.5em, 15vw, 20em);
			height: clamp(6.5em, 15vw, 20em);

			@media (width <= 768px) {
				margin-left: -1rem;
			}
		}

		& h1 {
			color: hsl(0, 0%, 0%);
			font-family: var(--sans-bold);
			font-size: clamp(1.25rem, 5vw, 4rem);
			font-weight: 800;
			letter-spacing: 2px;
			margin: 0;
			padding: 0;

			@media (width <= 768px) {
				margin-left: -1rem;
			}
		}

		@media (width <= 768px) {
			gap: 0.5rem;
			margin-left: -1rem;
		}
	}
</style>
