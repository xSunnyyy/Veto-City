<script>
	import { Nav, Footer } from "$lib/components";
	import { dev } from '$app/environment';
	import { injectAnalytics } from '@vercel/analytics/sveltekit';

	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<main class="page-wrapper">
	<Nav />

	<slot />

	<Footer />
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #111; /* dark background */
		font-family: system-ui, sans-serif;
	}

	.page-wrapper {
		background-color: #111;
		min-height: 100vh;
		padding: 2rem 1rem;
	}

	/* Optional: make all text white by default */
	:global(h1),
	:global(h2),
	:global(h3),
	:global(h4),
	:global(h5),
	:global(h6),
		color: white;
	}
</style>
