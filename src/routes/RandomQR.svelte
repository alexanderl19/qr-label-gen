<script lang="ts" module>
	export let codes = $state<string[]>([]);
</script>

<script lang="ts">
	import { customAlphabet } from 'nanoid';
	import qrcode from 'qrcode-generator';

	interface Props {
		qrSize: number;
		parentWidth: number;
		baseUrl: string;
		rowNumber: number;
		columnNumber: number;
	}

	let { qrSize, parentWidth, baseUrl, rowNumber, columnNumber }: Props = $props();

	const nanoid = customAlphabet('0123456789abcdefghijklmnopqrstuvwxyz', 10);

	const code = nanoid();
	codes.push(code);

	const qr = qrcode(0, 'Q');
	qr.addData(`${baseUrl}${code}`);
	qr.make();

	const svg = qr.createSvgTag({
		cellSize: 1,
		margin: 4,
		scalable: true
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
