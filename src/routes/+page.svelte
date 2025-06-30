<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import Delete from '@lucide/svelte/icons/delete';

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

	let attempts = $state([]);
	let current = $state(0);
	let currentText = $state([]) as string[];
</script>

{currentText}

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
	<div class="grid h-fit w-fit grid-cols-5 gap-2">
		{#each Array(30)}
			<div class="size-15 rounded border"></div>
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
			<Button class="h-full w-20">Enter</Button>
			{#each es.slice(21, es.length) as key}
				{@render Key(key)}
			{/each}
			<Button
				class="h-full w-20"
				onclick={() => {
					currentText.pop();
				}}
			>
				<Delete />
			</Button>
		</div>
	</div>
</div>
