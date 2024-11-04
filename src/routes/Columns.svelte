<script lang="ts" module>
	export let codes = $state<string[]>([]);
</script>

<script lang="ts">
	import type { ItemsType } from './Config.svelte';
	import Columns from './Columns.svelte';
	import QR from '@svelte-put/qr/svg/QR.svelte';
	import { customAlphabet } from 'nanoid';

	interface Props {
		columns: ItemsType;
		qrSize: number;
		parentWidth: number;
		baseUrl: string;
	}

	let { columns, qrSize, parentWidth, baseUrl }: Props = $props();

	const genId = customAlphabet('0123456789abcdefghijklmnopqrstuvwxyz', 10);
	const code = genId();
	codes.push(code);
</script>

<div class="cols">
	{#each columns as column}
		{#if column._type === 'nested'}
			<div style:width="{(column.size / parentWidth) * 100}%">
				<Columns columns={column.items} {qrSize} parentWidth={column.size} {baseUrl} />
			</div>
		{:else if column._type === 'qr'}
			<div style:width="{(qrSize / parentWidth) * 100}%">
				<QR data="{baseUrl}{code}" errorCorrectionLevel="L" />
			</div>
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
