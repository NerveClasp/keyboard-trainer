<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	const onKeyDown = (e) => {
		console.log(e);
	};

	onMount(() => {
		if (typeof window !== 'undefined') {
			document.addEventListener('keydown', onKeyDown);
		}
	});
	onDestroy(() => {
		if (typeof window !== 'undefined') {
			document.removeEventListener('keydown', onKeyDown);
		}
	});
</script>

<h1>about</h1>
