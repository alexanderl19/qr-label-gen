<script lang="ts">
	import { Copy } from 'lucide-svelte';

	interface Props {
		oncopy: () => void;
	}

	let { oncopy }: Props = $props();
</script>

<button class="delete" onclick={() => oncopy()}>
	<Copy size={14} />
</button>

<style lang="scss">
	.delete {
		all: unset;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 22px;
		width: 32px;
		background: #f0f0f0;
		border: 1px solid #d9d9d9;
		border-radius: 1000px;
		cursor: default;

		&:hover {
			border: 1px solid #cecece;
			background: #e8e8e8;
		}
	}
</style>
