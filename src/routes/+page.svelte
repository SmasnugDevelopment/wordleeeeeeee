<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import Delete from '@lucide/svelte/icons/delete';
	import clsx from 'clsx';

	const WORD_LENGTH = 5;

	let es = [
		'E',
		'Ē',
		'É',
		'Ě',
		'È',
		'Ê',
		'Ę',
		'Ë',
		'Ė',
		'Ẹ',
		'Ẽ',
		'Ḗ',
		'Ể',
		'Ẻ',
		'Ệ',
		'Ế',
		'Ễ',
		'Є',
		'Ё̄',
		'Ҽ',
		'ⱻ',
		'€',
		'℮',
		'ⅇ',
		'𝔼',
		'∈',
		'ⓔ',
		'📧',
		'🅴',
		'🄴'
	];

	let word = $state('');
	for (let i = 0; i < WORD_LENGTH; i++) {
		word += es[Math.floor(Math.random() * es.length)];
	}

	let attempts = $state([]) as string[][];
	let current = $state(0) as number;
	let currentText = $state([]) as string[];
</script>

{#snippet Key(key: string)}
	<Button
		variant="outline"
		class="block h-12 w-10 p-0 text-xl"
		onclick={() => {
			if (currentText.length !== 5) {
				currentText.push(key);
			}
		}}>{key}</Button
	>
{/snippet}

<div class="flex h-screen w-screen flex-col items-center justify-center gap-10">
	{word}
	<div class={clsx('grid h-fit w-fit gap-2', 'grid-cols-' + WORD_LENGTH)}>
		{#each Array(30) as _, index}
			<div class="flex size-15 items-center justify-center rounded border text-2xl">
				{Math.floor(index / WORD_LENGTH) == current
					? currentText[index - Math.floor(index / WORD_LENGTH) * WORD_LENGTH]
					: attempts[Math.floor(index / WORD_LENGTH)] &&
						attempts[Math.floor(index / WORD_LENGTH)][
							index - Math.floor(index / WORD_LENGTH) * WORD_LENGTH
						]}
			</div>
		{/each}
	</div>
	<div class="flex flex-col items-center gap-2">
		<div class="flex flex-row gap-2">
			{#each es.slice(0, 11) as key}
				{@render Key(key)}
			{/each}
		</div>

		<div class="flex flex-row gap-2">
			{#each es.slice(11, 21) as key}
				{@render Key(key)}
			{/each}
		</div>

		<div class="flex flex-row gap-2">
			<Button
				class="h-full w-20"
				disabled={currentText.length !== WORD_LENGTH}
				onclick={() => {
					if (currentText.length === WORD_LENGTH) {
						attempts.push(currentText);
						currentText = [];
						current++;
					}
				}}>Enter</Button
			>
			{#each es.slice(21, es.length) as key}
				{@render Key(key)}
			{/each}
			<Button
				class="h-full w-20"
				disabled={currentText.length === 0}
				variant="destructive"
				onclick={() => {
					currentText.pop();
				}}
			>
				<Delete />
			</Button>
		</div>
	</div>
</div>
