<script lang="ts">
	import { Trash2 } from 'lucide-svelte';

	interface Props {
		ondelete: () => void;
	}

	let { ondelete }: Props = $props();
</script>

<button class="delete" onclick={() => ondelete()}>
	<Trash2 size={14} />
</button>

<style lang="scss">
	.delete {
		all: unset;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 22px;
		width: 32px;
		background: #feebec;
		border: 1px solid #fdbdbe;
		border-radius: 1000px;
		cursor: default;

		&:hover {
			border: 1px solid #f4a9aa;
			background: #ffdbdc;
		}
	}
</style>
