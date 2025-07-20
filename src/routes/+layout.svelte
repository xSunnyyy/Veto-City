<script>
	import { Nav, Footer } from "$lib/components";
	import { dev } from '$app/environment';
	import { injectAnalytics } from '@vercel/analytics/sveltekit';

	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #111; /* global black background */
		font-family: system-ui, sans-serif;
	}

	:global(h1),
	:global(h3),
	:global(h5),
	:global(input),
	:global(span) {
		color: white;
	}

	.page-wrapper {
		background-color: #111;
		min-height: 100vh;
		padding: 2rem 1rem;
	}
</style>

<main>
	<Nav />

	<div class="page-wrapper">
		<slot />
	</div>

	<Footer />
</main>

