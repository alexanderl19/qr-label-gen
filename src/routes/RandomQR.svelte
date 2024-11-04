<script lang="ts" module>
	export let codes = $state<string[]>([]);
</script>

<script lang="ts">
	import QR from '@svelte-put/qr/svg/QR.svelte';
	import { customAlphabet } from 'nanoid';

	interface Props {
		qrSize: number;
		parentWidth: number;
		baseUrl: string;
	}

	let { qrSize, parentWidth, baseUrl }: Props = $props();

	const nanoid = customAlphabet('0123456789abcdefghijklmnopqrstuvwxyz', 10);

	const code = nanoid();
	codes.push(code);
</script>

<div style:width="{(qrSize / parentWidth) * 100}%" data-id={code}>
	<QR data="{baseUrl}{code}" errorCorrectionLevel="L" />
</div>
