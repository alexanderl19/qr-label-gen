<script lang="ts">
	import type { ItemsType } from './Config.svelte';
	import Row from './Rows.svelte';

	interface Props {
		rows: ItemsType;
		columns: ItemsType;
		qrSize: number;
	}

	let { rows, columns, qrSize }: Props = $props();

	const width = 600;
</script>

<div class="page">
	<Row {rows} {columns} {qrSize} {width} />
</div>

<style lang="scss">
	.page {
		aspect-ratio: 8.5/11;
		width: 600px;
		background-color: #fcfcfc;
		overflow: hidden;
	}
</style>
