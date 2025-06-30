<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';

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
</script>

{#snippet Key(key: string)}
	<Button>{key}</Button>
{/snippet}

<div class="flex h-screen w-screen flex-col items-center justify-center">
	<div class="grid h-fit w-fit grid-cols-5 gap-2">
		{#each Array(30)}
			<div class="size-15 rounded border"></div>
		{/each}
	</div>
	<div class="flex flex-row">
		{#each es.slice(0, 11) as key}
			{@render Key(key)}
		{/each}
	</div>

	<div class="flex flex-row">
		{#each es.slice(11, 21) as key}
			{@render Key(key)}
		{/each}
	</div>

	<div class="flex flex-row">
		{#each es.slice(21, es.length) as key}
			{@render Key(key)}
		{/each}
	</div>
</div>
