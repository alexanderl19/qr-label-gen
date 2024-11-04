<script lang="ts">
	import type { Snippet } from 'svelte';
	import { dragHandle } from 'svelte-dnd-action';
	import { GripVertical } from 'lucide-svelte';

	interface Props {
		children?: Snippet;
		actions?: Snippet;
		name: string;
		borderRadius?: number;
	}

	let { children, actions, name, borderRadius = 4 }: Props = $props();
</script>

<div class="item" style:--item-border-radius="{borderRadius}px">
	<div class="content">
		{#if children}
			{@render children()}
		{/if}
	</div>
	<div class="divider"></div>
	<div class="handle" use:dragHandle>
		<div class="grip">
			<GripVertical size={16} />
		</div>
		<span>{name}</span>
		<div class="actions">
			{#if actions}
				{@render actions()}
			{/if}
		</div>
	</div>
</div>

<style lang="scss">
	.item {
		flex-grow: 1;
		min-width: 90px;
		height: 100%;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		float: left;
		border: 1px solid #d9d9d9;
		border-radius: var(--item-border-radius);
		overflow: hidden;
		background: #fff;
	}

	.content {
		flex-grow: 1;
	}

	.divider {
		width: 100%;
		height: 1px;
		background-color: #d9d9d9;
	}

	.handle {
		background-color: #f9f9f9;
		display: flex;
		align-items: center;
		padding: 2px;

		.grip {
			display: flex;
			align-items: center;
		}
	}

	.actions {
		flex-grow: 1;
		display: flex;
		justify-content: flex-end;
		padding-left: 24px;
	}
</style>
