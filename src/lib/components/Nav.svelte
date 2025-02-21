<script>
	import Title from '$lib/components/Title.svelte';

	let isMenuOpen = $state(false);

	const navItems = [
		{ href: '/', label: 'Home' },
		{ href: '/about', label: 'About' }
	];

	function toggleMenu() {
		isMenuOpen = !isMenuOpen;
	}
</script>

<nav>
	<div class="nav-container">
		<div class="nav-content">
			<Title />

			<div class="desktop-menu">
				{#each navItems as { href, label }}
					<a {href} class="nav-link">
						{label}
					</a>
				{/each}
			</div>

			<button
				class="burger"
				onclick={toggleMenu}
				aria-label="Toggle menu"
				class:active={isMenuOpen}
			>
				<span class="bar"></span>
				<span class="bar"></span>
				<span class="bar"></span>
			</button>
		</div>
	</div>

	<div class="mobile-menu {isMenuOpen ? 'show' : ''}">
		{#each navItems as { href, label }}
			<a {href} class="mobile-nav-link" onclick={toggleMenu}>
				{label}
			</a>
		{/each}
	</div>
</nav>

<style>
	nav {
		background-color: white;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
		position: relative;
	}

	.nav-container {
		max-width: 1200px;
		margin-inline: auto;
		padding: 0 1rem;
	}

	.nav-content {
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 4em;
		padding: 1.5rem 1rem;
	}

	.desktop-menu {
		display: none;
	}

	.nav-link {
		padding: 0.5rem 0.75rem;
		text-decoration: none;
		color: hsl(0, 0%, 31%);
		font-size: clamp(1.25rem, 3vw, 4rem);
		font-weight: 900;
		letter-spacing: 5px;
	}

	.nav-link:hover {
		background-color: hsl(0, 0%, 31%);
		color: #f0f0f0;
		border-radius: 5px;
		padding: 0.1rem 0.75rem;
	}

	.mobile-menu {
		display: none;
		position: fixed;
		top: 5.5em;
		left: 0;
		right: 0;
		width: 100%;
		height: calc(100vh - 4em);
		background-color: #f0f0f0;
		margin: 0;
		padding: 0;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 3em;
		z-index: 50;
	}

	.mobile-menu.show {
		display: flex;
	}

	/* Burger Menu */
	.burger {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		width: 2rem;
		height: 1.5rem;
		background: transparent;
		border: none;
		cursor: pointer;
		padding: 0;
		z-index: 51;
		position: relative;
	}

	.bar {
		width: 100%;
		height: 5px;
		background-color: hsl(0, 0%, 31%);
		transition: all 0.3s ease-in-out;
		transform-origin: center;
		border-radius: 5px;
	}

	.burger.active .bar:first-child {
		transform: translateY(10px) rotate(45deg);
		background-color: hsl(0, 0%, 0%);
	}

	.burger.active .bar:nth-child(2) {
		opacity: 0;
	}

	.burger.active .bar:last-child {
		transform: translateY(-10px) rotate(-45deg);
		background-color: hsl(0, 0%, 0%);
	}

	.mobile-nav-link {
		display: block;
		padding: 0.75rem 0;
		text-decoration: none;
		color: hsl(0, 0%, 31%);
		font-family: var(--sans-bold);
		font-size: clamp(2rem, 3vw, 2rem);
		font-weight: 900;
		letter-spacing: 10px;
		border-radius: 0.25rem;
		text-align: center;
		width: 100%;
		margin: 0;
	}

	.mobile-nav-link:hover {
		background-color: hsl(0, 0%, 31%);
		color: #f0f0f0;
	}

	@media (min-width: 768px) {
		.desktop-menu {
			display: flex;
			align-items: center;
		}

		.burger {
			display: none;
		}

		.mobile-menu {
			display: none !important;
		}

		.nav-content {
			height: 9em;
		}

		.nav-link {
			margin-right: 1em;
		}
	}

	@media (width > 768px) {
		:global(.burger) {
			display: none;
		}
	}
</style>
