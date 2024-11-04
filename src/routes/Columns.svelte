<script lang="ts">
	import type { ItemsType } from './Config.svelte';
	import Columns from './Columns.svelte';
	import QR from '@svelte-put/qr/svg/QR.svelte';

	interface Props {
		columns: ItemsType;
		qrSize: number;
		parentWidth: number;
	}

	let { columns, qrSize, parentWidth }: Props = $props();
</script>

<div class="cols">
	{#each columns as column}
		{#if column._type === 'nested'}
			<div style:width="{(column.size / parentWidth) * 100}%">
				<Columns columns={column.items} {qrSize} parentWidth={column.size} />
			</div>
		{:else if column._type === 'qr'}
			<div style:width="{(qrSize / parentWidth) * 100}%">
				<QR data="https://svelte-put.vnphanquang.com/docs/qr" />
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
