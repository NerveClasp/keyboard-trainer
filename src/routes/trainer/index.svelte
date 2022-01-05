<script lang="ts">
	import Keyboard from '$lib/keyboard/Keyboard.svelte';
	import Input from '$lib/input/Input.svelte';
	import { currentIndex } from './_stores.js';
	import { onMount, onDestroy } from 'svelte';

	let currentIndexValue = 0;
	const unsubscribe = currentIndex.subscribe((value) => {
		currentIndexValue = value;
	});
	const text = 'mmm train me please lol';
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
		currentKey = text[index];
	};

	$: if (changed) {
		if (userKey === 'Backspace') {
			console.log(textArray[currentIndexValue - 1]);
			textArray[currentIndexValue - 1].status = 'erased';
			currentIndex.set(currentIndexValue - 1);
			changed = false;
		} else {
			const expectedKey = textArray[currentIndexValue].key;
			const success = expectedKey === userKey;
			textArray[currentIndexValue].status = success ? 'success' : 'mistake';
			let newIndex = currentIndexValue + 1;

			if (success) {
				updateCurrentKey(newIndex);
				currentIndex.set(newIndex);
			} else if (text[currentIndexValue] !== ' ') {
				currentIndex.set(newIndex);
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
