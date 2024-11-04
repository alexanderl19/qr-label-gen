<script lang="ts" module>
	type SpacerItem = {
		_type: 'spacer';
		id: string;
		weight: number;
	};
	type QRItem = {
		_type: 'qr';
		id: string;
	};
	type NestedItem = {
		_type: 'nested';
		id: string;
		items: (SpacerItem | QRItem)[];
	};

	export type ItemType = SpacerItem | QRItem | NestedItem;
	export type Items = ItemType[];
</script>

<script lang="ts">
	import Item from './Item.svelte';
	import { flip } from 'svelte/animate';
	import { dndzone, dragHandleZone } from 'svelte-dnd-action';
	import Spacer from './Spacer.svelte';
	import Qr from './QR.svelte';

	import Bank from './Bank.svelte';

	interface Props {
		items: Items;
	}

	let { items = $bindable() }: Props = $props();

	const flipDurationMs = 200;
</script>

<section
	class="config"
	use:dragHandleZone={{ items, flipDurationMs, centreDraggedOnCursor: true, dropTargetStyle: {} }}
	onconsider={(e) => {
		items = e.detail.items;
	}}
	onfinalize={(e) => {
		items = e.detail.items;
	}}
>
	{#each items as item (item.id)}
		<div animate:flip={{ duration: flipDurationMs }}>
			{#if item._type === 'spacer'}
				<Spacer bind:weight={item.weight} />
			{:else if item._type === 'qr'}
				<Qr />
			{:else if item._type === 'nested'}
				<Item name="Group">
					{#snippet actions()}
						<Bank type={item.id} />
					{/snippet}
					<div
						class="nested"
						use:dndzone={{
							items: item.items,
							type: item.id,
							flipDurationMs,
							dropTargetStyle: {}
						}}
						onconsider={(e) => {
							const id = items.findIndex((c) => c.id === item.id);
							(items[id] as NestedItem).items = e.detail.items;
						}}
						onfinalize={(e) => {
							const id = items.findIndex((c) => c.id === item.id);
							(items[id] as NestedItem).items = e.detail.items;
						}}
					>
						{#each item.items as subitem (subitem.id)}
							<div class="card" animate:flip={{ duration: flipDurationMs }}>
								{#if subitem._type === 'spacer'}
									<Spacer bind:weight={subitem.weight} borderRadius={2} />
								{:else if subitem._type === 'qr'}
									<Qr borderRadius={2} />
								{/if}
							</div>
						{/each}
					</div>
				</Item>
			{/if}
		</div>
	{/each}
</section>
<div class="add">
	<Bank includeNested={true} />
</div>

<style lang="scss">
	.config {
		display: flex;
		gap: 12px;
	}

	.nested {
		display: flex;
		gap: 8px;
		padding: 2px;
		box-sizing: border-box;
		height: 100%;
	}

	.add {
		margin: 6px 0 0 0;
		width: max-content;
		position: sticky;
		left: 8px;
	}
</style>
