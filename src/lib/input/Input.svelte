<script lang="ts">
	import { onDestroy, onMount } from 'svelte';

	let text = 'something to type';
	let currentKey = '';

	const onKeyPress = ({ key }) => {
		currentKey = key;
	};
	onMount(() => {
		if (typeof window !== 'undefined') {
			document.addEventListener('keypress', onKeyPress);
		}
	});
	onDestroy(() => {
		if (typeof window !== 'undefined') {
			document.removeEventListener('keypress', onKeyPress);
		}
	});
</script>

<div>
	{#each text as char}
		<span class={`char${char === currentKey ? ' active' : ''}`}>{char}</span>
	{/each}
</div>

<style>
	.active {
		color: var(--accent-color);
	}
</style>
