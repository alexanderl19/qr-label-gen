<script lang="ts">
	import Item from './Item.svelte';

	interface Props {
		weight: number;
		borderRadius?: number;
	}

	let { weight = $bindable(), borderRadius }: Props = $props();
</script>

<Item name="Spacer" {borderRadius}>
	<input class="spacer-weight" type="number" min="1" bind:value={weight} />
</Item>

<style lang="scss">
	.spacer-weight {
		width: 100%;
		height: 100%;
		box-sizing: border-box;
		border: none;
		text-align: center;
		font-size: 1rem;
		padding: 8px;
	}
</style>
