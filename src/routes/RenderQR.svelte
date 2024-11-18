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
		qrColor: string;
	}

	let { qrSize, parentWidth, baseUrl, rowNumber, columnNumber, code, margin, qrColor }: Props =
		$props();

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
	class="qr"
	style:width="{(qrSize / parentWidth) * 100}%"
	data-url="{baseUrl}{code}"
	data-row={rowNumber}
	data-column={columnNumber}
	style:--qr-color={qrColor}
>
	{@html svg}
</div>

<style lang="scss">
	.qr :global(svg) {
		& :global(rect) {
			display: none;
		}

		& :global(path) {
			fill: var(--qr-color, '#000');
		}
	}
</style>
