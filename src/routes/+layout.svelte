<script>
	import { injectAnalytics } from '@vercel/analytics/sveltekit';
	injectAnalytics();

	import Footer from '$lib/Footer.svelte';
	let { children } = $props();
	import '../app.css';
</script>

{@render children()}
<Footer />
