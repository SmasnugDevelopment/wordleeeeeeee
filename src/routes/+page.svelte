<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import Delete from '@lucide/svelte/icons/delete';
	import clsx from 'clsx';

	const WORD_LENGTH = Math.round(Math.E);
	const ATTEMPTS = 10;
	const LETTERS = [
		'E',
		'Ē',
		'É',
		'Ě',
		'Ę',
		'Ė',
		'Ẹ',
		'Ẽ',
		'Є',
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

	let word = $state([]) as string[];
	for (let i = 0; i < WORD_LENGTH; i++) {
		word.push(LETTERS[Math.floor(Math.random() * LETTERS.length)]);
	}

	let attempts = $state([]) as string[][];
	let current = $state(0) as number;
	let currentText = $state([]) as string[];
</script>

{#snippet Key(key: string)}
	{#key key}
		<Button
			variant="outline"
			class="block h-12 w-10 p-0 text-xl"
			onclick={() => {
				if (currentText.length !== WORD_LENGTH) {
					currentText.push(key);
				}
			}}>{key}</Button
		>
	{/key}
{/snippet}

<svelte:window
	onkeypress={(event) => {
		if (event.key == 'e') {
			if (currentText.length !== WORD_LENGTH) {
				currentText.push('E');
			}
		}
	}}
/>

<svelte:head>
	<title>Wordleeeeeeee</title>
</svelte:head>

<div class="flex h-screen w-screen flex-col items-center justify-center gap-10">
	<div class="flex h-fit w-fit flex-col gap-2">
		{#each Array(ATTEMPTS) as _, attempt}
			<div class="flex flex-row gap-2">
				{#each Array(WORD_LENGTH) as _, letter}
					<div
						class={clsx(
							'flex size-15 items-center justify-center rounded border text-2xl',
							attempts[attempt] && attempts[attempt][letter] === word[letter] && 'bg-green-400/25',
							attempts[attempt] &&
								attempts[attempt][letter] !== word[letter] &&
								word.includes(attempts[attempt][letter]) &&
								'bg-yellow-400/25',

							attempts[attempt] && attempts[attempt][letter] !== word[letter] && 'bg-gray-400/25'
						)}
					>
						{attempt == current
							? currentText[letter]
							: attempts[attempt] && attempts[attempt][letter]}
					</div>
				{/each}
			</div>
		{/each}
	</div>
	<div class="flex flex-col items-center gap-2">
		<div class="flex flex-row gap-2">
			{#each LETTERS.slice(0, 7) as key}
				{@render Key(key)}
			{/each}
		</div>

		<div class="flex flex-row gap-2">
			{#each LETTERS.slice(7, 15) as key}
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
			{#each LETTERS.slice(15, LETTERS.length) as key}
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
