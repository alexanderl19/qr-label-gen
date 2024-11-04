<script lang="ts">
	import QR from '@svelte-put/qr/svg/QR.svelte';
	import { nanoid } from 'nanoid';
	import Config, { type Items } from './Config.svelte';

	let rowCol = $state<'row' | 'col'>('row');

	const defaultState = [
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
	];
	let rows = $state<Items>(defaultState);
	let cols = $state<Items>(defaultState);

	$inspect(rows);
</script>

<div class="configure">
	<form class="row-col-selector">
		<label>
			Rows
			<input type="radio" name="row-cols" value="row" bind:group={rowCol} />
		</label>
		<label>
			Columns
			<input type="radio" name="row-cols" value="col" bind:group={rowCol} />
		</label>
	</form>
	<div class="from-to">
		<div class="from">
			{#if rowCol === 'row'}
				Left
			{:else}
				Top
			{/if}
		</div>
		<div class="line"></div>
		<div class="to">
			{#if rowCol === 'row'}
				Right
			{:else}
				Bottom
			{/if}
		</div>
	</div>
	<div class="content">
		{#if rowCol === 'row'}
			<Config bind:items={rows} />
		{:else}
			<Config bind:items={cols} />
		{/if}
	</div>
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
	.row-col-selector {
		padding: 8px 0;
		position: sticky;
		left: 8px;
		width: max-content;
		display: flex;
		gap: 12px;

		label {
			width: max-content;
			display: flex;
			gap: 3px;
		}
	}

	.configure {
		margin: 24px 0;
	}

	.content {
		padding: 0 8px;
	}

	.from-to {
		width: 100vw;
		padding: 4px 8px;
		box-sizing: border-box;
		position: sticky;
		left: 0;
		display: flex;
		align-items: center;
		gap: 6px;

		.from,
		.to {
			font-size: 14px;
			color: #646464;
		}

		.line {
			flex-grow: 1;
			height: 1px;
			background-color: #d9d9d9;
		}
	}
</style>
