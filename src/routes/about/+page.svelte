<script>
</script>

<article>
	<div class="inner">
		<h1>About this website</h1>

		<p>
			This is a color palette generator but it will show you the colors of an image that you
			provide.
		</p>

		<p>This project was built with the new svelte 5 syntax!</p>
	</div>

	<a href="/">Home</a>
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

			& p {
				color: rgb(75, 75, 75);
				font-family: var(--sans);
				font-size: clamp(1.25rem, 2vw, 1.5rem);
				font-weight: 600;
			}
		}

		& a {
			color: rgb(70, 70, 70);
			text-decoration: inherit;
			font-family: var(--sans-bold);
			font-size: clamp(1rem, 3vw, 2rem);
			font-weight: 800;
			letter-spacing: 3px;
			position: absolute;
			bottom: 0;
			left: 50%;
			transform: translateX(-50%);

			&:hover {
				text-decoration: underline;
			}
		}
	}
</style>
