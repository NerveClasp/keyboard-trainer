<script lang="ts">
	import Keyboard from '$lib/keyboard/Keyboard.svelte';
	import Input from '$lib/input/Input.svelte';
</script>

<div class="content">
	<p>Trainer</p>
	<div><Input /></div>
	<div><Keyboard /></div>
</div>

<style>
	.content {
		width: 100%;
		max-width: var(--column-width);
		margin: var(--column-margin-top) auto 0 auto;
	}
</style>
