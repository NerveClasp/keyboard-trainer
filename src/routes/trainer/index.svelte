<script lang="ts">
	import Keyboard from '$lib/keyboard/Keyboard.svelte';
	import Input from '$lib/input/Input.svelte';
	import { currentIndex } from './_stores.js';
	import { onMount, onDestroy } from 'svelte';

	let currentIndexValue = 0;
	const unsubscribe = currentIndex.subscribe((value) => {
		currentIndexValue = value;
	});
	const text = 'third stream is done';
	type Key = {
		key: string;
		status: 'default' | 'success' | 'mistake' | 'erased';
		irrelevant?: boolean;
	};
	let textArray: Key[] = text.split('').map((key) => ({ key, status: 'default' }));
	let userKey = '';
	let currentKey = text[currentIndexValue];
	const expectedInputRegex = /^(?:[\w ]|Backspace)$/gi;
	let changed = false;
	const MAX_USER_ERROR_KEYS = 20;
	let userErrors = 0;
	let done = false;

	const onKeyDown = ({ key }) => {
		if (expectedInputRegex.test(key)) {
			userKey = key;
			changed = true;
		}
	};

	const onKeyUp = ({ key }) => {
		if (expectedInputRegex.test(key)) {
			userKey = '';
		}
	};

	const updateCurrentKey = (index: number) => {
		if (index >= 0 && index < textArray.length + 1) {
			currentKey = text[index];
		}
	};

	const updateIndex = (index: number) => {
		if (index >= 0 && index < textArray.length + 1) {
			currentIndex.set(index);
		}
	};

	$: if (changed && !done && currentIndexValue <= textArray.length) {
		if (userKey === 'Backspace') {
			const newIndex = currentIndexValue - 1;
			textArray[newIndex].status = 'erased';
			updateIndex(newIndex);
			if (textArray[newIndex].irrelevant) {
				textArray.splice(newIndex, 1);
			} else {
				updateCurrentKey(newIndex);
			}

			changed = false;
		} else {
			const { key } = textArray?.[currentIndexValue] ?? {};
			const success = key === userKey;
			if (key) {
				textArray[currentIndexValue].status = success ? 'success' : 'mistake';
			}
			let newIndex = currentIndexValue + 1;

			if (success) {
				updateCurrentKey(newIndex);
				updateIndex(newIndex);
				if (newIndex === textArray.length) {
					done = true;
				}
			} else if ((key === ' ' || !key) && userErrors < MAX_USER_ERROR_KEYS) {
				textArray.splice(currentIndexValue, 0, {
					key: userKey,
					status: 'mistake',
					irrelevant: true
				});
				userErrors += 1;
				updateIndex(newIndex);
			} else if (userErrors < MAX_USER_ERROR_KEYS) {
				updateIndex(newIndex);
				updateCurrentKey(newIndex);
			}

			changed = false;
		}
	}

	onMount(() => {
		textArray = text.split('').map((key) => ({ key, status: 'default' }));

		if (typeof window !== 'undefined') {
			document.addEventListener('keydown', onKeyDown);
			document.addEventListener('keydown', onKeyUp);
		}
	});
	onDestroy(() => {
		if (typeof window !== 'undefined') {
			document.removeEventListener('keydown', onKeyDown);
			document.removeEventListener('keydown', onKeyUp);
		}
		unsubscribe();
	});
</script>

<div class="content">
	<p>Trainer</p>
	<div><Input currentIndex={currentIndexValue} {textArray} /></div>
	<div><Keyboard {currentKey} /></div>
</div>

<style>
	.content {
		width: 100%;
		max-width: var(--column-width);
		margin: var(--column-margin-top) auto 0 auto;
	}
</style>
