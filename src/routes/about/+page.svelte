<script>
	import SEO from '$lib/components/SEO.svelte';
</script>

<SEO
	title="About PixiPalette"
	description="What is PixiPalette? A web app that extracts the dominant colors from an image."
	keywords="PixiPalette, about pixipalette"
/>

<article>
	<div class="inner">
		<h1>About PixiPalette</h1>

		<h3>Discover your perfect palette</h3>

		<p>
			Have you ever stumbled upon a color in an image and wondered, "What color value is that?"
			PixiPalette is here to answer that question. This web app allows you to upload an image and
			extracts the dominant colors from it, providing you with three different color values. It's a
			handy tool for designers, artists, or anyone who appreciates color.
		</p>

		<p class="second">
			PixiPalette is a simple, efficient, and free tool that helps you find the colors you love in
			any image. Whether you're a designer looking for inspiration, an artist seeking the perfect
			shade, or just curious about the colors in your favorite photo, PixiPalette makes it easy to
			discover and use color palettes from images.
		</p>
	</div>
</article>

<style>
	article {
		text-align: center;

		& .inner {
			background-color: #fff;
			box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
			width: fit-content;
			height: auto;
			margin-inline: auto;
			padding: 2rem;
			border-radius: 9px;
			margin-top: 2em;

			& h1 {
				color: hsl(0, 0%, 41%);
				font-family: var(--sans-bold);
				font-size: clamp(1.5rem, 6vw, 8rem);
				font-weight: 800;
				letter-spacing: 2px;
				margin: 0;
			}

			& h3 {
				color: rgb(0, 0, 0);
				font-family: var(--sans);
				font-size: clamp(1.15rem, 4vw, 2.75rem);
				font-weight: 600;
				margin-block: -0.5rem;
				padding: 0;
			}

			& p {
				color: hsl(0, 0%, 0%);
				font-family: var(--sans);
				font-size: clamp(1.25rem, 3vw, 1.75rem);
				font-weight: 600;
				letter-spacing: 2px;
				max-width: 90%;
				margin-inline: auto;
				margin-top: 5em;
			}

			& .second {
				margin-top: 1rem;
			}
		}
	}
</style>
