<script lang="ts">
	import { flip } from 'svelte/animate';
	import { type ItemType, type ItemsType } from './Config.svelte';

	import {
		dndzone,
		TRIGGERS,
		SHADOW_ITEM_MARKER_PROPERTY_NAME,
		type DndZoneAttributes
	} from 'svelte-dnd-action';
	import { nanoid } from 'nanoid';

	import Add from './Add.svelte';

	interface Props {
		includeNested?: boolean;
		type?: string;
	}

	let { includeNested = false, type }: Props = $props();

	let items = $state<ItemsType>([
		{ _type: 'spacer' as const, id: nanoid(), weight: 1 },
		{ _type: 'qr' as const, id: nanoid() },
		...(includeNested
			? [
					{
						_type: 'nested' as const,
						id: nanoid(),
						items: [],
						size: 2.5
					}
				]
			: [])
	]);

	const flipDurationMs = 300;
	let shouldIgnoreDndEvents = $state(false);

	const handleDndConsider: DndZoneAttributes<ItemType>['onconsider'] = (e) => {
		const { trigger, id } = e.detail.info;

		if (trigger === TRIGGERS.DRAG_STARTED) {
			const idx = items.findIndex((item) => item.id === id);
			const newId = nanoid();

			// @ts-ignore `SHADOW_ITEM_MARKER_PROPERTY_NAME` managed by svelte-dnd-action
			e.detail.items = e.detail.items.filter((item) => !item[SHADOW_ITEM_MARKER_PROPERTY_NAME]);
			e.detail.items.splice(idx, 0, { ...items[idx], id: newId });
			items = e.detail.items;
			shouldIgnoreDndEvents = true;
		} else if (!shouldIgnoreDndEvents) {
			items = e.detail.items;
		} else {
			items = [...items];
		}
	};

	const handleDndFinalize: DndZoneAttributes<ItemType>['onfinalize'] = (e) => {
		if (!shouldIgnoreDndEvents) {
			items = e.detail.items;
		} else {
			items = [...items];
			shouldIgnoreDndEvents = false;
		}
	};
</script>

<section
	class="bank"
	use:dndzone={{
		items,
		flipDurationMs,
		centreDraggedOnCursor: true,
		dropFromOthersDisabled: true,
		type,
		dropTargetStyle: {}
	}}
	onconsider={handleDndConsider}
	onfinalize={handleDndFinalize}
>
	{#each items as item (item.id)}
		<div animate:flip={{ duration: flipDurationMs }}>
			<Add type={item._type} />
		</div>
	{/each}
</section>

<style lang="scss">
	.bank {
		display: flex;
		align-items: stretch;
		gap: 2px;
	}
</style>
