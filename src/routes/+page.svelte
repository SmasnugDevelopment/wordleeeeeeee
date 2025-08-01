<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import Delete from '@lucide/svelte/icons/delete';
	import clsx from 'clsx';
	import * as AlertDialog from '$lib/components/ui/alert-dialog/index.js';
	import * as Dialog from '$lib/components/ui/dialog/index.js';

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
	function setup() {
		for (let i = 0; i < WORD_LENGTH; i++) {
			word.push(LETTERS[Math.floor(Math.random() * LETTERS.length)]);
		}
	}
	setup();

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
		if (event.key === 'Enter') {
			if (currentText.length === WORD_LENGTH) {
				attempts.push(currentText);
				currentText = [];
				current++;
			}
		} else if (LETTERS.includes(event.key.toUpperCase())) {
			if (currentText.length !== WORD_LENGTH) {
				currentText.push(event.key.toUpperCase());
			}
		}
	}}
/>

<svelte:head>
	<title>Wordleeeeeeee</title>
</svelte:head>

<AlertDialog.Root
	open={attempts.length !== 0 &&
		attempts[attempts.length - 1][0] == word[0] &&
		attempts[attempts.length - 1][1] == word[1] &&
		attempts[attempts.length - 1][2] == word[2]}
>
	<AlertDialog.Content>
		<AlertDialog.Header>
			<AlertDialog.Title>eeeeeeeeeeeeeeee</AlertDialog.Title>
			<AlertDialog.Description>
				You found the word in {attempts.length} attempts!
			</AlertDialog.Description>
		</AlertDialog.Header>
		<AlertDialog.Footer>
			<Button
				onclick={() => {
					attempts = [];
					currentText = [];
					current = 0;
					word = [];
					setup();
				}}>Retry</Button
			>
		</AlertDialog.Footer>
	</AlertDialog.Content>
</AlertDialog.Root>

<div class="flex h-screen w-screen flex-col items-center p-5">
	<div class="relative mt-auto w-full overflow-auto">
		<div class="mx-auto flex h-fit w-fit flex-col gap-2 pb-5">
			{#each Array(ATTEMPTS) as _, attempt}
				<div class="flex flex-row gap-2">
					{#each Array(WORD_LENGTH) as _, letter}
						<div
							class={clsx(
								'flex size-15 items-center justify-center rounded border text-2xl',
								attempts[attempt] &&
									attempts[attempt][letter] === word[letter] &&
									'bg-green-400/25',
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
	</div>
	<div
		class="to-background pointer-events-none h-10 w-full -translate-y-5 bg-gradient-to-b from-transparent"
	></div>
	<div class="mb-auto flex flex-col items-center gap-2">
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
	<Dialog.Root>
		<Dialog.Trigger class="m-3">How to play</Dialog.Trigger>
		<Dialog.Content>
			<Dialog.Header>
				<Dialog.Title>How to play</Dialog.Title>
				<Dialog.Description>
					You have 10 attempts to guess a 3 (thats e rounded to the nearest whole number) character
					sequence of variations of the letter e. You are not without help. If a letter is in the
					sequence, it will be highlighted in yellow. If a letter is in the correct place in the
					sequence. If a letter is not in the sequence, it will be highlighted grey.
				</Dialog.Description>
			</Dialog.Header>
		</Dialog.Content>
	</Dialog.Root>
</div>
