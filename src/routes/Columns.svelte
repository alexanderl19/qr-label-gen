<script lang="ts">
	import type { ItemsType } from './Config.svelte';
	import Columns from './Columns.svelte';
	import RandomQr from './RandomQR.svelte';

	interface Props {
		columns: ItemsType;
		qrSize: number;
		parentWidth: number;
		baseUrl: string;
		rowNumber: number;
		qrColumns: Record<string, number>;
	}

	let { columns, qrSize, parentWidth, baseUrl, rowNumber, qrColumns }: Props = $props();
</script>

<div class="cols">
	{#each columns as column}
		{#if column._type === 'nested'}
			<div style:width="{(column.size / parentWidth) * 100}%">
				<Columns
					columns={column.items}
					{qrSize}
					parentWidth={column.size}
					{baseUrl}
					{rowNumber}
					{qrColumns}
				/>
			</div>
		{:else if column._type === 'qr'}
			{@const columnNumber = qrColumns[column.id]}
			<RandomQr {qrSize} {parentWidth} {baseUrl} {rowNumber} {columnNumber} />
		{:else if column._type === 'spacer'}
			<div style:flex-grow={column.weight}></div>
		{/if}
	{/each}
</div>

<style lang="scss">
	.cols {
		display: flex;
		flex-basis: 0;
		height: 100%;
	}
</style>
