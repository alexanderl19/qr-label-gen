<script lang="ts">
	import { Space, QrCode, Group } from 'lucide-svelte';

	interface Props {
		type: 'spacer' | 'qr' | 'nested';
	}

	let { type }: Props = $props();


</script>

<button class="delete" onclick={}> </button>

<style lang="scss">
	.delete {
		all: unset;
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
