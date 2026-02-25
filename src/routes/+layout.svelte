<script lang="ts">
	import './layout.css';
	import documentIcon from '$lib/assets/document-gray.png';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={documentIcon} /></svelte:head>
{@render children()}
