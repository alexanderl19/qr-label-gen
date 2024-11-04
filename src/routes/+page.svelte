<script lang="ts">
	import { nanoid } from 'nanoid';
	import Config, { type ItemsType } from './Config.svelte';
	import Preview from './Preview.svelte';
	import { ssp, queryParameters } from 'sveltekit-search-params';

	let rowCol = $state<'row' | 'col'>('row');

	const defaultState: ItemsType = [
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
			],
			size: 2.5
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
			],
			size: 2.5
		},
		{ _type: 'spacer' as const, id: nanoid(), weight: 1 }
	];
	const params = queryParameters({
		qrSize: ssp.number(1),
		rows: ssp.lz(defaultState),
		columns: ssp.lz(defaultState)
	});
	let rows = $state<ItemsType>($params.rows);
	let columns = $state<ItemsType>($params.columns);
	let qrSize = $state($params.qrSize);
	$effect(() => {
		$params.rows = $state.snapshot(rows);
	});
	$effect(() => {
		$params.columns = $state.snapshot(columns);
	});
	$effect(() => {
		$params.qrSize = $state.snapshot(qrSize);
	});

	$inspect(rows);
</script>

<div class="configure">
	<label class="paper-size">
		Paper Size
		<select>
			<option value="letter">Letter (8.5 x 11in)</option>
		</select>
	</label>
	<label class="paper-size">
		QR Size
		<input type="number" name="qr-size" bind:value={qrSize} />
	</label>
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
				Top
			{:else}
				Left
			{/if}
		</div>
		<div class="line"></div>
		<div class="to">
			{#if rowCol === 'row'}
				Bottom
			{:else}
				Right
			{/if}
		</div>
	</div>
	<div class="content">
		{#if rowCol === 'row'}
			<Config bind:items={rows} />
		{:else}
			<Config bind:items={columns} />
		{/if}
	</div>
</div>

<Preview {rows} {columns} {qrSize} />

<style lang="scss">
	.configure {
		margin: 24px 0;
	}

	.paper-size {
		position: sticky;
		left: 8px;
		display: flex;
		width: max-content;
		align-items: baseline;
		gap: 12px;
	}

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
