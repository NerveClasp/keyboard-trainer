<script lang="ts">
	type Key = {
		key: string;
		status: string;
		irrelevant?: boolean;
	};
	export let textArray: Key[] = [{ key: ' ', status: 'default' }];
	export let currentIndex = 0;
</script>

<div class="text-wrapper">
	{#each textArray as { key, status, irrelevant }, i}
		<span
			class={`char ${status !== 'default' ? status : ''}${key === ' ' ? ' space' : ''}`}
			class:active={i === currentIndex}
			class:irrelevant
		>
			{key}
		</span>
	{/each}
</div>

<style>
	.text-wrapper {
		display: flex;
	}
	.char {
		position: relative;
	}
	.active::before {
		position: absolute;
		left: -1px;
		top: 0;
		content: '';
		width: 1px;
		height: 100%;
		background-color: var(--accent-color);
		animation: cursor-blink 0.6s ease-in-out infinite;
	}

	@keyframes -global-cursor-blink {
		0% {
			opacity: 0;
		}
		50% {
			opacity: 1;
		}
		100% {
			opacity: 0;
		}
	}

	.success {
		color: var(--key-success);
	}
	.mistake {
		color: var(--key-mistake);
	}
	.erased {
		color: var(--key-erased);
	}
	.irrelevant {
		opacity: 0.5;
	}
	.space {
		display: inline-block;
		width: 1ch;
	}
</style>
