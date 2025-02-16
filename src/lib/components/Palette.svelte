<script>
	let imageUrl = $state('');
	let colors = $state([]);
	let imageInput;

	function handleImageUpload(event) {
		const file = event.target.files[0];

		if (file) {
			const reader = new FileReader();

			reader.onload = (e) => {
				imageUrl = e.target.result;
				extractColors(e.target.result);
			};

			reader.readAsDataURL(file);
		}
	}

	function extractColors(imageSrc) {
		const img = new Image();
		img.src = imageSrc;

		img.onload = () => {
			const canvas = document.createElement('canvas');
			const ctx = canvas.getContext('2d');
			canvas.width = img.width;
			canvas.height = img.height;

			ctx.drawImage(img, 0, 0);

			const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height).data;
			const colorMap = new Map();

			// Sample colors with better spacing
			for (let i = 0; i < imageData.length; i += 20) {
				const r = Math.floor(imageData[i] / 16) * 16;
				const g = Math.floor(imageData[i + 1] / 16) * 16;
				const b = Math.floor(imageData[i + 2] / 16) * 16;

				// Skip very dark and very light colors
				if (r + g + b < 60 || r + g + b > 740) continue;

				// Calculate color difference to ensure colors are distinct
				const rgb = `rgb(${r},${g},${b})`;
				let isDistinct = true;

				for (let existingColor of colorMap.keys()) {
					const [er, eg, eb] = existingColor.match(/\d+/g).map(Number);
					const colorDiff = Math.sqrt(
						Math.pow(r - er, 2) + Math.pow(g - eg, 2) + Math.pow(b - eb, 2)
					);

					if (colorDiff < 60) {
						// Adjust this threshold to control color distinction
						isDistinct = false;
						break;
					}
				}

				if (isDistinct) {
					colorMap.set(rgb, (colorMap.get(rgb) || 0) + 1);
				}
			}

			// Get top 5 most common colors
			colors = Array.from(colorMap.entries())
				.sort((a, b) => b[1] - a[1])
				.slice(0, 5)
				.map((entry) => entry[0]);
		};
	}

	function getContrastColor(rgb) {
		// Extract RGB values from the rgb string
		const [r, g, b] = rgb.match(/\d+/g).map(Number);

		// Calculate relative luminance using the formula
		const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;

		// Return white for dark colors, black for light colors
		return luminance > 0.5 ? '#000000' : '#ffffff';
	}
</script>

<div class="wrapper">
	<div class="inner">
		<input
			type="file"
			accept="image/*"
			bind:this={imageInput}
			onchange={handleImageUpload}
			class="file-input"
		/>

		{#if imageUrl}
			<img src={imageUrl} alt="Uploaded item" class="preview-image" />
		{/if}

		<div class="color-palette">
			{#each colors as color}
				<div class="color-box" style:background-color={color}>
					<span class="color-value" style:color={getContrastColor(color)}>{color}</span>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	:global(body) {
		background-color: #fff;
	}

	.wrapper {
		padding: 1.2rem;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 1.2rem;
		background-color: #fff;
		min-height: 100dvh;
	}

	.inner {
		display: flex;
		flex-direction: column;
		gap: 1.2rem;
		align-items: center;
		justify-content: center;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
		padding: 0.5rem 1rem;
		max-width: 600px;
		margin-bottom: 18em;
	}

	.preview-image {
		max-width: 100%;
		height: auto;
		max-height: 400px;
		object-fit: contain;
	}

	.color-palette {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
		gap: 0.6rem;
		width: 100%;
		padding: 0.5rem;
		box-sizing: border-box;
	}

	.color-box {
		width: 100%;
		height: 100%;
		border-radius: 8px;
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
		aspect-ratio: 1;
	}

	.color-value {
		padding: 0.25rem 0.75rem;
		border-radius: 4px;
		font-family: var(--sans);
		font-size: clamp(0.8rem, 2.5vw, 1.35rem);
		text-shadow: none;
		width: 3em;
		height: 3em;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 5rem;
	}

	.file-input {
		color: rgb(43, 42, 42);
		padding: 0.5rem 0.75rem;
		padding-bottom: 1rem;
		margin-bottom: 1.2rem;
		border: none;
		font-family: var(--sans);
		font-size: clamp(0.875rem, 1.5vw, 1.22rem);
		width: 100%;
		max-width: 100%;
		box-sizing: border-box;

		&::file-selector-button {
			margin-right: 1em;
			padding: 0.5rem 1rem;
			border: none;
			border-radius: 5px;
			background-color: #f0f0f0;
			cursor: pointer;
			font-size: clamp(0.875rem, 1.5vw, 1.22rem);
			white-space: nowrap;

			&:hover {
				background-color: #e0e0e0;
			}
		}
	}

	@media (max-width: 480px) {
		.wrapper {
			padding: 0.8rem;
		}

		.inner {
			padding: 0.5rem;
		}

		.color-palette {
			grid-template-columns: repeat(2, 1fr);
		}

		.color-value {
			font-size: 0.75rem;
		}
	}
</style>
