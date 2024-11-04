<script lang="ts">
	import type { ItemsType } from './Config.svelte';
	import Row from './Rows.svelte';

	interface Props {
		rows: ItemsType;
		columns: ItemsType;
		qrSize: number;
		baseUrl: string;
	}

	let { rows, columns, qrSize, baseUrl }: Props = $props();
</script>

<div class="page">
	<Row {rows} {columns} {qrSize} parentWidth={8.5} parentHeight={11} {baseUrl} />
</div>

<style lang="scss">
	.page {
		aspect-ratio: 8.5/11;
		width: 600px;
		background-color: #fcfcfc;
		overflow: hidden;

		@media print {
			background-color: transparent;
			width: 100%;
		}
	}

	@media print {
		:global(body) {
			margin: 0;
		}
	}

	@page {
		size: 8.5in 11in;
		margin: 0;
	}
</style>
