<script lang="ts">
	import Item from './Item.svelte';
	import { QrCode } from 'lucide-svelte';

	interface Props {
		borderRadius?: number;
	}

	let { borderRadius }: Props = $props();
</script>

<Item name="QR" {borderRadius}>
	<div class="qr">
		<QrCode size={20} />
	</div>
</Item>

<style lang="scss">
	.qr {
		width: 100%;
		height: 100%;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
