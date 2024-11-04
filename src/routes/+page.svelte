<script lang="ts">
	import QR from '@svelte-put/qr/svg/QR.svelte';
	import { nanoid } from 'nanoid';
	import Config, { type Items } from './Config.svelte';

	let rows = $state<Items>([
		{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
		{
			_type: 'nested' as const,
			id: nanoid(),
			items: [
				{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
				{ _type: 'qr' as const, id: nanoid() },
				{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
				{ _type: 'qr' as const, id: nanoid() },
				{ _type: 'spacer' as const, id: nanoid(), weight: 1 }
			]
		},
		{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
		{
			_type: 'nested' as const,
			id: nanoid(),
			items: [
				{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
				{ _type: 'qr' as const, id: nanoid() },
				{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
				{ _type: 'qr' as const, id: nanoid() },
				{ _type: 'spacer' as const, id: nanoid(), weight: 1 }
			]
		},
		{ _type: 'spacer' as const, id: nanoid(), weight: 1 }
	]);

	$inspect(rows);
</script>

<div class="preview">
	<div class="label">Rows</div>
	<Config bind:items={rows} />
</div>

<!-- Columns -->

<!-- <div class="rows">
	{#each rowsParsed as row}
		{#if row === ' '}
			<div class="columns">
				{#each columnsParsed as column}
					{#if column === 'qr'}
						<QR
							data="https://svelte-put.vnphanquang.com/docs/qr"
							moduleFill="violet"
							anchorOuterFill="red"
							anchorInnerFill="violet"
						/>
					{:else if typeof column === 'number'}
						<div style:flex-weight={column}></div>
					{/if}
				{/each}
			</div>
		{:else if typeof row === 'number'}
			<div style:flex-weight={row}></div>
		{/if}
	{/each}
</div> -->

<style lang="scss">
	.label {
		margin: 8px 0;
	}

	.preview {
		margin: 0 8px 24px 8px;
	}
</style>
