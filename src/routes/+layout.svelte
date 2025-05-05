<script>
	import Footer from '$lib/Footer.svelte';
	let { children } = $props();
	import '../app.css';
</script>

{@render children()}
<Footer />
