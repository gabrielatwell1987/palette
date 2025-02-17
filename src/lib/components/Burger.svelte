<script>
	let isOpen = $state(false);
	let onToggle = $state();

	function toggleMenu() {
		isOpen = !isOpen;
		console.log('Burger clicked, isOpen:', isOpen);
		if (onToggle) onToggle(isOpen);
	}
</script>

<button class="burger" aria-label="Menu" aria-expanded={isOpen} onclick={toggleMenu}>
	<div class="burger-lines">
		<span class="line {isOpen ? 'open' : ''}"></span>
		<span class="line {isOpen ? 'open' : ''}"></span>
		<span class="line {isOpen ? 'open' : ''}"></span>
	</div>
</button>

<style>
	.burger {
		position: relative;
		display: block;
		padding: 0.5rem;
		background: none;
		border: none;
		cursor: pointer;
	}

	.burger-lines {
		width: 2rem;
		height: 1.5rem;
		position: relative;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}

	.line {
		display: block;
		height: 0.3rem;
		width: 100%;
		background-color: #000;
		transition:
			transform 0.5s,
			opacity 0.5s;
		border: 1px solid red;
	}

	.line.open:nth-child(1) {
		transform: translateY(9px) rotate(45deg);
	}

	.line.open:nth-child(2) {
		opacity: 0;
	}

	.line.open:nth-child(3) {
		transform: translateY(-9px) rotate(-45deg);
	}

	@media (min-width: 768px) {
		.burger {
			display: none;
		}
	}
</style>
