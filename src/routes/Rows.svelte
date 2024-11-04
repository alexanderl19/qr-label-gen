<script lang="ts">
	import Columns from './Columns.svelte';
	import type { ItemsType } from './Config.svelte';
	import Row from './Rows.svelte';

	interface Props {
		rows: ItemsType;
		columns: ItemsType;
		qrSize: number;
		width: number;
	}

	let { rows, columns, qrSize, width }: Props = $props();
</script>

<div class="rows">
	{#each rows as row}
		{#if row._type === 'nested'}
			<div style:height="{(row.size / 8.5) * width}px">
				<Row rows={row.items} {columns} {qrSize} {width} />
			</div>
		{:else if row._type === 'qr'}
			<div style:height="{(qrSize / 8.5) * width}px">
				<Columns {columns} {qrSize} {width} />
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
