<script lang="ts">
	import type { ItemsType } from './Config.svelte';
	import Columns from './Columns.svelte';
	import RenderQr from './RenderQR.svelte';

	interface Props {
		columns: ItemsType;
		qrSize: number;
		parentWidth: number;
		baseUrl: string;
		rowNumber: number;
		qrColumns: Record<string, number>;
		ids: string[];
		width: number;
		margin: number;
	}

	let { columns, qrSize, parentWidth, baseUrl, rowNumber, qrColumns, ids, width, margin }: Props =
		$props();
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
					{ids}
					{width}
					{margin}
				/>
			</div>
		{:else if column._type === 'qr'}
			{@const columnNumber = qrColumns[column.id]}
			<RenderQr
				{qrSize}
				{parentWidth}
				{baseUrl}
				{rowNumber}
				{columnNumber}
				code={ids[rowNumber * width + columnNumber]}
				{margin}
			/>
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
