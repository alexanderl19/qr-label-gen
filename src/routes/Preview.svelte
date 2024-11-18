<script lang="ts">
	import { customAlphabet } from 'nanoid';
	import type { ItemsType } from './Config.svelte';
	import Row from './Rows.svelte';

	interface Props {
		rows: ItemsType;
		columns: ItemsType;
		qrSize: number;
		baseUrl: string;
		codes?: string[];
		margin: number;
		backgroundColor: string;
	}

	let {
		rows,
		columns,
		qrSize,
		baseUrl,
		codes: bindableCodes = $bindable(),
		margin,
		backgroundColor
	}: Props = $props();

	const itemsToQrMap = (items: ItemsType) =>
		Object.fromEntries(
			items
				.filter((item) => item._type === 'qr' || item._type === 'nested')
				.reduce(
					(acc, current) => {
						const previousIndex = acc.at(-1)?.[1] ?? -1;

						if (current._type === 'qr') {
							acc.push([current.id, previousIndex + 1]);
						} else if (current._type === 'nested') {
							const qrItems = current.items.filter((item) => item._type === 'qr');
							if (current.syncQr === true) {
								qrItems.forEach((item) => {
									acc.push([item.id, previousIndex + 1]);
								});
							} else {
								qrItems.forEach((item, i) => {
									acc.push([item.id, previousIndex + 1 + i]);
								});
							}
						}

						return acc;
					},
					[] as [string, number][]
				)
		);

	const qrRowMap = $derived(itemsToQrMap(rows));
	const qrColumnMap = $derived(itemsToQrMap(columns));
	const qrRowCount = $derived(Math.max(...Object.values(qrRowMap)) + 1);
	const qrColumnCount = $derived(Math.max(...Object.values(qrColumnMap)) + 1);
	const qrCount = $derived(qrRowCount * qrColumnCount);

	$inspect(qrRowCount, qrColumnCount, qrCount);

	const nanoid = customAlphabet('0123456789abcdefghijklmnopqrstuvwxyz', 10);

	const codes = $derived(
		Array(qrCount)
			.fill(undefined)
			.map(() => nanoid())
	);
	$inspect(codes);

	$effect(() => {
		bindableCodes = codes;
	});

	$inspect(qrRowMap);
	$inspect(qrColumnMap);
</script>

<div class="page" style:background={backgroundColor}>
	<Row
		{rows}
		{columns}
		{qrSize}
		parentWidth={8.5}
		parentHeight={11}
		{baseUrl}
		qrRows={qrRowMap}
		qrColumns={qrColumnMap}
		ids={codes}
		width={qrColumnCount}
		{margin}
	/>
</div>

<style lang="scss">
	.page {
		aspect-ratio: 8.5/11;
		width: 600px;
		background-color: var(--background, #fcfcfc);
		overflow: hidden;

		@media print {
			background-color: var(--background, transparent);
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
