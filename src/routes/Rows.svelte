<script lang="ts">
	import Columns from './Columns.svelte';
	import type { ItemsType } from './Config.svelte';
	import Row from './Rows.svelte';

	interface Props {
		rows: ItemsType;
		columns: ItemsType;
		qrSize: number;
		parentWidth: number;
		parentHeight: number;
	}

	let { rows, columns, qrSize, parentWidth, parentHeight }: Props = $props();
</script>

<div class="rows">
	{#each rows as row}
		{#if row._type === 'nested'}
			<div style:height="{(row.size / parentHeight) * 100}%">
				<Row rows={row.items} {columns} {qrSize} {parentWidth} parentHeight={row.size} />
			</div>
		{:else if row._type === 'qr'}
			<div style:height="{(qrSize / parentHeight) * 100}%">
				<Columns {columns} {qrSize} parentWidth={8.5} />
			</div>
		{:else if row._type === 'spacer'}
			<div style:flex-grow={row.weight}></div>
		{/if}
	{/each}
</div>

<style lang="scss">
	.rows {
		display: flex;
		height: 100%;
		flex-direction: column;
		flex-basis: 0;
	}
</style>
