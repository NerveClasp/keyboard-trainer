<script lang="ts">
	export let currentKey = '';

	const colemakDHm = [
		[
			// left side
			[
				{ key: 'q', finger: 'l4' },
				{ key: 'w', finger: 'l3' },
				{ key: 'f', finger: 'l2' },
				{ key: 'p', finger: 'l1' },
				{ key: 'b', finger: 'l1' }
			],
			[
				{ key: 'a', finger: 'l4' },
				{ key: 'r', finger: 'l3' },
				{ key: 's', finger: 'l2' },
				{ key: 't', finger: 'l1' },
				{ key: 'g', finger: 'l1' }
			],
			[
				{ key: 'z', finger: 'l4' },
				{ key: 'x', finger: 'l3' },
				{ key: 'c', finger: 'l2' },
				{ key: 'd', finger: 'l1' },
				{ key: 'v', finger: 'l1' }
			]
		],
		// right side
		[
			[
				{ key: 'j', finger: 'r1' },
				{ key: 'l', finger: 'r1' },
				{ key: 'u', finger: 'r2' },
				{ key: 'y', finger: 'r3' },
				{ key: ';', finger: 'r4' }
			],
			[
				{ key: 'm', finger: 'r1' },
				{ key: 'n', finger: 'r1' },
				{ key: 'e', finger: 'r2' },
				{ key: 'i', finger: 'r3' },
				{ key: 'o', finger: 'r4' }
			],
			[
				{ key: 'k', finger: 'r1' },
				{ key: 'h', finger: 'r1' },
				{ key: ',', finger: 'r2' },
				{ key: '.', finger: 'r3' },
				{ key: '/', finger: 'r4' }
			]
		]
	];
</script>

<div class="keyboard">
	{#each colemakDHm as side}
		<div class="side">
			{#each side as row}
				<div class="row">
					{#each row as { key }}
						<span class={`key${key === currentKey ? ' active' : ''}`}>
							<span class="letter">{key}</span>
						</span>
					{/each}
				</div>
			{/each}
		</div>
	{/each}
</div>

<style>
	.row,
	.keyboard {
		display: flex;
	}
	.row {
		height: 6vh;
	}
	.side {
		padding: 12px;
	}

	.key {
		--key-size: calc(5vh * 0.75);
		display: flex;
		align-items: center;
		justify-content: center;
		width: var(--key-size);
		height: var(--key-size);
		border: 1px solid var(--accent-color);
		border-bottom-right-radius: 7%;
		border-bottom-left-radius: 7%;
		padding: 4px;
		margin: 1px;
		color: var(--accent-color);
	}

	.active {
		--active: #fff;
		color: var(--active);
		border-color: var(--active);
	}

	.letter {
		display: block;
		font-size: 2.2vh;
		font-weight: bold;
		text-transform: uppercase;
	}
</style>
