<script lang="ts">
	import { Space, QrCode, Group } from 'lucide-svelte';

	interface Props {
		type: 'spacer' | 'qr' | 'nested';
	}

	let { type }: Props = $props();
</script>

<div class="add">
	{#if type === 'spacer'}
		<Space size={14} />
	{:else if type === 'qr'}
		<QrCode size={14} />
	{:else if type === 'nested'}
		<Group size={14} />
	{/if}
</div>

<style lang="scss">
	.add {
		display: flex;
		align-items: center;
		justify-content: center;
		height: 22px;
		width: 32px;
		background: #fff;
		border: 1px dashed #d9d9d9;
		border-radius: 1000px;
	}
</style>
