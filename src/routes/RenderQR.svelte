<script lang="ts" module>
	export let codes = $state<string[]>([]);
</script>

<script lang="ts">
	import qrcode from 'qrcode-generator';

	interface Props {
		qrSize: number;
		parentWidth: number;
		baseUrl: string;
		rowNumber: number;
		columnNumber: number;
		code: string;
		margin: number;
	}

	let { qrSize, parentWidth, baseUrl, rowNumber, columnNumber, code, margin }: Props = $props();

	const svg = $derived.by(() => {
		const qr = qrcode(0, 'Q');
		qr.addData(`${baseUrl}${code}`);
		qr.make();

		return qr.createSvgTag({
			cellSize: 1,
			margin,
			scalable: true
		});
	});
</script>

<div
	style:width="{(qrSize / parentWidth) * 100}%"
	data-url="{baseUrl}{code}"
	data-row={rowNumber}
	data-column={columnNumber}
>
	{@html svg}
</div>
