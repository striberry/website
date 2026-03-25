<script lang="ts">
	// this was hard :(
	const blobs = [
		{ cx: '10%', cy: '10%', r: 80, dur: 18 },
		{ cx: '50%', cy: '10%', r: 40, dur: 22 },
		{ cx: '17%', cy: '15%', r: 70, dur: 16 },
		{ cx: '90%', cy: '20%', r: 120, dur: 25 },
		{ cx: '30%', cy: '25%', r: 30, dur: 20 },
		{ cx: '50%', cy: '60%', r: 80, dur: 19 },
		{ cx: '70%', cy: '90%', r: 50, dur: 23 },
		{ cx: '90%', cy: '60%', r: 90, dur: 17 },
		{ cx: '30%', cy: '90%', r: 80, dur: 21 },
		{ cx: '15%', cy: '40%', r: 60, dur: 24 },
		{ cx: '60%', cy: '30%', r: 45, dur: 15 },
		{ cx: '80%', cy: '75%', r: 70, dur: 26 },
		{ cx: '40%', cy: '50%', r: 100, dur: 20 },
		{ cx: '25%', cy: '70%', r: 35, dur: 18 },
		{ cx: '75%', cy: '45%', r: 55, dur: 22 },
		{ cx: '55%', cy: '80%', r: 75, dur: 19 },
		{ cx: '5%', cy: '65%', r: 65, dur: 23 },
		{ cx: '45%', cy: '20%', r: 50, dur: 16 },
		{ cx: '12%', cy: '85%', r: 60, dur: 21 },
		{ cx: '65%', cy: '15%', r: 35, dur: 17 },
		{ cx: '85%', cy: '35%', r: 95, dur: 24 },
		{ cx: '20%', cy: '55%', r: 45, dur: 19 },
		{ cx: '95%', cy: '80%', r: 70, dur: 27 }
	];
</script>

<div class="bg"></div>

<svg class="blob-cont" aria-hidden="true">
	<defs>
		<filter
			id="gooey"
			x="-20%"
			y="-20%"
			width="140%"
			height="140%"
			color-interpolation-filters="sRGB"
		>
			<feGaussianBlur in="SourceGraphic" stdDeviation="18" result="blur" />
			<feColorMatrix
				in="blur"
				mode="matrix"
				values="1 0 0 0 0
				        0 1 0 0 0
				        0 0 1 0 0
				        0 0 0 22 -9"
				result="goo"
			/>
		</filter>

		<mask id="blob-mask" x="0" y="0" width="100%" height="100%">
			<g style="filter: url(#gooey)">
				{#each blobs as blob, i (i)}
					<circle
						class="blob"
						style="--i: {i}; --dur: {blob.dur}s; --start: {i * (360 / blobs.length)}deg;"
						cx={blob.cx}
						cy={blob.cy}
						r={blob.r}
						fill="white"
					/>
				{/each}
			</g>
		</mask>
	</defs>

	<rect
		x="0"
		y="0"
		width="100%"
		height="100%"
		fill="var(--accent)"
		filter="brightness(0.8)"
		mask="url(#blob-mask)"
	/>
</svg>

<style>
	.bg,
	.blob-cont {
		position: fixed;
		inset: 0;
		width: 100%;
		height: 100%;
		pointer-events: none;
	}

	.bg {
		z-index: -2;
		background: #040404;
	}

	.blob-cont {
		z-index: -1;
		opacity: 0.55;
	}

	.blob {
		animation: orbit var(--dur, 20s) infinite linear;
		animation-delay: calc(var(--i, 0) * -1.4s);
		transform-box: fill-box;
		transform-origin: center;
	}

	@keyframes orbit {
		from {
			transform: rotate(var(--start, 0deg)) translate(200px) rotate(calc(-1 * var(--start, 0deg)));
		}
		to {
			transform: rotate(calc(var(--start, 0deg) + 360deg)) translate(200px)
				rotate(calc(-1 * (var(--start, 0deg) + 360deg)));
		}
	}
</style>
