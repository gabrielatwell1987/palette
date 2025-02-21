<script>
	let imageUrl = $state('');
	let colors = $state([]);
	let imageInput;
	let colorFormat = $state('rgb');

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

	function convertColor(rgbStr, format) {
		// Extract RGB values
		const [r, g, b] = rgbStr.match(/\d+/g).map(Number);

		switch (format) {
			case 'hex':
				return `#${r.toString(16).padStart(2, '0')}${g.toString(16).padStart(2, '0')}${b.toString(16).padStart(2, '0')}`;
			case 'hsl':
				// Convert RGB to HSL
				const r1 = r / 255;
				const g1 = g / 255;
				const b1 = b / 255;

				const max = Math.max(r1, g1, b1);
				const min = Math.min(r1, g1, b1);
				let h,
					s,
					l = (max + min) / 2;

				if (max === min) {
					h = s = 0;
				} else {
					const d = max - min;
					s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
					switch (max) {
						case r1:
							h = (g1 - b1) / d + (g1 < b1 ? 6 : 0);
							break;
						case g1:
							h = (b1 - r1) / d + 2;
							break;
						case b1:
							h = (r1 - g1) / d + 4;
							break;
					}
					h /= 6;
				}

				return `hsl(${Math.round(h * 360)}, ${Math.round(s * 100)}%, ${Math.round(l * 100)}%)`;
			default:
				return rgbStr;
		}
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

		<select class="format-select" bind:value={colorFormat}>
			<option value="rgb">RGB</option>
			<option value="hex">HEX</option>
			<option value="hsl">HSL</option>
		</select>

		<div class="color-palette">
			{#each colors as color}
				<div class="color-box" style:background-color={color}>
					<span class="color-value" style:color={getContrastColor(color)}>
						{convertColor(color, colorFormat)}
					</span>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	:global(body) {
		background-color: #f0f0f0;
	}

	.wrapper {
		padding: 1.2rem;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 1.2rem;
		/* background-color: #f0f0f0; */
		min-height: 100dvh;
		width: 100%;
	}

	.inner {
		display: flex;
		flex-direction: column;
		gap: 1.2rem;
		align-items: center;
		justify-content: center;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
		padding: 0.5rem 1rem;
		width: clamp(250px, 90vw, 1000px);
		margin-bottom: 18em;
		background-color: #fff;
	}

	.preview-image {
		max-width: 100%;
		height: auto;
		max-height: 400px;
		object-fit: contain;
	}

	.color-palette {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
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
		font-size: clamp(1.1rem, 2.5vw, 1.35rem);
		text-shadow: none;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 5rem;
		text-align: center;
		width: auto;
		height: auto;
		word-break: break-all;
	}

	.file-input {
		color: hsl(0, 0%, 41%);
		padding: 0.5rem 0.75rem;
		padding-bottom: 1rem;
		margin-bottom: 1.2rem;
		border: none;
		font-family: var(--sans);
		font-size: clamp(0.875rem, 1.5vw, 1.22rem);
		width: 100%;
		max-width: 100%;
		box-sizing: border-box;
		cursor: pointer;

		&::file-selector-button {
			margin-right: 1em;
			padding: 0.5rem 1rem;
			border: none;
			border-radius: 5px;
			background-color: #f0f0f0;
			color: hsl(0, 0%, 31%);
			cursor: pointer;
			font-size: clamp(0.875rem, 1.5vw, 1.22rem);
			white-space: nowrap;

			&:hover {
				background-color: #e0e0e0;
			}
		}
	}

	.format-select {
		padding: 0.5rem;
		border: 1px solid #e0e0e0;
		border-radius: 4px;
		font-family: var(--sans-bold);
		font-size: clamp(1rem, 2vw, 1.5rem);
		letter-spacing: 5px;
		background-color: transparent;
		cursor: pointer;
		width: 100%;
		max-width: 200px;
		margin-bottom: 0.5rem;
		color: #242424;

		&:hover {
			border-color: #bdbdbd;
		}

		&:focus {
			outline: none;
			border-color: #242424;
			box-shadow: 0 0 0 2px rgba(36, 36, 36, 0.1);

			&:hover {
				border-color: #242424;
			}
		}

		& option {
			padding: 0.5rem;
			font-family: var(--sans);
		}
	}

	@media (max-width: 500px) {
		.wrapper {
			padding: 0.8rem;
			width: 99%;
			position: absolute;
			left: 50%;
			transform: translateX(-50%);
		}

		.inner {
			padding: 0.5rem;
			margin-inline: auto;
		}

		.color-palette {
			grid-template-columns: repeat(2, 1fr);
			padding: 0;
			width: 100%;
		}

		/* .color-value {
			font-size: 0.75rem;
		} */
	}
</style>
