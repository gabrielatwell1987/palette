<script>
</script>

<article>
	<div class="inner">
		<h1>About PixiPalette</h1>

		<h3>Discover your perfect palette</h3>

		<p>
			Have you ever stumbled upon a color in an image and wondered, "What rgb color is that?"
			PixiPalette is here to answer that question. This web app allows you to upload an image and
			extracts the dominant colors from it, providing you with their hex values. It's a handy tool
			for designers, artists, or anyone who appreciates color.
		</p>

		<p>
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
				color: rgb(59, 59, 59);
				font-family: var(--sans-bold);
				font-size: clamp(1.5rem, 6vw, 5rem);
				font-weight: 800;
				letter-spacing: 2px;
			}

			& h3 {
				color: rgb(75, 75, 75);
				font-family: var(--sans);
				font-size: clamp(1.15rem, 1vw, 1.75rem);
				font-weight: 600;
				margin: 0;
				padding: 0;
			}

			& p {
				color: rgb(75, 75, 75);
				font-family: var(--sans);
				font-size: clamp(1.25rem, 2vw, 1.5rem);
				font-weight: 600;
				width: 75%;
				margin-inline: auto;
			}
		}
	}
</style>
