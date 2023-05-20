<script context="module" lang="ts">
	export type customRgb = {r: number, g: number, b: number};
</script>

<script lang="ts">
	import { onMount } from 'svelte';

	export let mask: string;
	export let rgb: customRgb;

	let canvas;

	onMount(() => {
		canvas.style.webkitMaskImage = `url(${mask})`;
    	canvas.style.maskImage = `url(${mask})`;
		canvas.style.webkitMaskPosition = 'center';
		canvas.style.maskPosition = 'center';
		canvas.style.webkitMaskRepeat = 'no-repeat';
		canvas.style.maskRepeat = 'no-repeat';

		const ctx = canvas.getContext('2d', { willReadFrequently: true });
		let frame;

		(function loop() {
			frame = requestAnimationFrame(loop);

			const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

			for (let p = 0; p < imageData.data.length; p += 4) {
				const i = p / 4;
				const x = i % canvas.width;
				const y = i / canvas.height >>> 0;

				const t = window.performance.now();

				const r = 64 + (rgb.r * x / canvas.width) + (64 * Math.sin(t / 1000));
				const g = 64 + (rgb.g * y / canvas.height) + (64 * Math.cos(t / 1400));

				imageData.data[p + 0] = r;
				imageData.data[p + 1] = g;
				imageData.data[p + 2] = rgb.b;
				imageData.data[p + 3] = 255;
			}

			ctx.putImageData(imageData, 0, 0);
		}());

		return () => {
			cancelAnimationFrame(frame);
		};
	});
</script>

<canvas
	bind:this={canvas}
></canvas>

<style>
	canvas {
		width: 100%;
		height: 100%;
		background-color: #666;
	}
</style>