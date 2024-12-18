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
		size: number;
		syncQr?: boolean;
	};

	export type ItemType = SpacerItem | QRItem | NestedItem;
	export type ItemsType = ItemType[];
</script>

<script lang="ts">
	import Item from './Item.svelte';
	import { flip } from 'svelte/animate';
	import { dndzone, dragHandleZone } from 'svelte-dnd-action';
	import Spacer from './Spacer.svelte';
	import Qr from './QR.svelte';

	import Bank from './Bank.svelte';
	import Delete from './Delete.svelte';
	import Copy from './Copy.svelte';
	import { nanoid } from 'nanoid';

	interface Props {
		items: ItemsType;
	}

	let { items = $bindable() }: Props = $props();

	const deleteById = (id: string, nestedId?: string) => {
		if (nestedId) {
			const nestedIdx = items.findIndex((i) => i.id === nestedId && i._type === 'nested');
			const nestedItem = items[nestedIdx] as NestedItem;
			const idx = nestedItem.items.findIndex((i) => i.id === id);
			(items[nestedIdx] as NestedItem).items.splice(idx, 1);
		} else {
			const idx = items.findIndex((i) => i.id === id);
			items.splice(idx, 1);
		}
		items = [...items];
	};

	const flipDurationMs = 200;
</script>

<section
	class="config"
	use:dragHandleZone={{ items, flipDurationMs, centreDraggedOnCursor: true, dropTargetStyle: {} }}
	onconsider={(e) => {
		items = e.detail.items;
		items = [...items];
	}}
	onfinalize={(e) => {
		items = e.detail.items;
		items = [...items];
	}}
>
	{#each items as item, i (item.id)}
		<div animate:flip={{ duration: flipDurationMs }}>
			{#if item._type === 'spacer'}
				<Spacer bind:weight={item.weight} ondelete={() => deleteById(item.id)} />
			{:else if item._type === 'qr'}
				<Qr ondelete={() => deleteById(item.id)} />
			{:else if item._type === 'nested'}
				<Item name="Group">
					{#snippet actions()}
						<div class="group-actions">
							<Bank type={item.id} />
							<Copy
								oncopy={() => {
									const itemToCopy = items[i] as NestedItem;
									items.splice(i + 1, 0, {
										...itemToCopy,
										id: nanoid(),
										items: itemToCopy.items.map((item) => ({ ...item, id: nanoid() }))
									});
									items = [...items];
								}}
							/>
							<Delete ondelete={() => deleteById(item.id)} />
						</div>
					{/snippet}
					<div>
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
								items = [...items];
							}}
							onfinalize={(e) => {
								const id = items.findIndex((c) => c.id === item.id);
								(items[id] as NestedItem).items = e.detail.items;
								items = [...items];
							}}
						>
							{#each item.items as subitem (subitem.id)}
								<div class="card" animate:flip={{ duration: flipDurationMs }}>
									{#if subitem._type === 'spacer'}
										<Spacer
											bind:weight={subitem.weight}
											borderRadius={2}
											ondelete={() => deleteById(subitem.id, item.id)}
										/>
									{:else if subitem._type === 'qr'}
										<Qr borderRadius={2} ondelete={() => deleteById(subitem.id, item.id)} />
									{/if}
								</div>
							{/each}
						</div>
						<div class="nested-options">
							<input type="checkbox" bind:checked={item.syncQr} />
							<input class="nested-size" type="number" min="1" bind:value={item.size} />
						</div>
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
		width: max-content;
	}

	.nested {
		display: flex;
		gap: 8px;
		padding: 2px;
		box-sizing: border-box;
		height: 100%;
	}

	.nested-options {
		display: flex;
		gap: 4px;

		.nested-size {
			width: 100%;
			height: 100%;
			box-sizing: border-box;
			border: none;
			text-align: center;
			font-size: 1rem;
			padding: 8px;
		}
	}

	.group-actions {
		display: flex;
		gap: 2px;
	}

	.add {
		margin: 6px 0 0 0;
		width: max-content;
		position: sticky;
		left: 8px;
	}
</style>
