<script lang="ts">
	let defaultFontSize: number = 16;
	let minViewportWidth: number = 320;
	let maxViewportWidth: number = 1280;
	let minFontSize: number = 1;
	let maxFontSize: number = 2;

	$: minViewportWidthInRem = minViewportWidth / defaultFontSize;
	$: maxViewportWidthInRem = maxViewportWidth / defaultFontSize;

	$: slope = (maxFontSize - minFontSize) / (maxViewportWidthInRem - minViewportWidthInRem);
	$: yAxisIntersection = -minViewportWidthInRem * slope + minFontSize;

	$: clampString = `clamp(${minFontSize}rem, ${yAxisIntersection.toFixed(4)}rem + ${(
		slope * 100
	).toFixed(4)}vw, ${maxFontSize}rem);`;
</script>

<header>
	<h1>Font Clamper</h1>
	<p>Calculate linearly scaling font size based on viewport width</p>
</header>

<main>
	<section class="caluculator flex">
		<label
			>Default font size in pixels:
			<input type="number" min="1" bind:value={defaultFontSize} />
		</label>
		<label
			>Minimum viewport width in pixels:
			<input type="number" min="1" bind:value={minViewportWidth} />
		</label>
		<label
			>Maximum viewport width in pixels:
			<input type="number" min="1" bind:value={maxViewportWidth} />
		</label>
		<label
			>Minimum font size in rem:
			<input type="number" min="1" bind:value={minFontSize} />
		</label>
		<label
			>Maximum font size in rem:
			<input type="number" min="1" bind:value={maxFontSize} />
		</label>
	</section>

	<section class="result">
		<code>{clampString}</code>
	</section>
</main>

<style>
	.flex {
		display: flex;
		flex-direction: column;
	}

	label {
		margin-bottom: 1rem;
	}
</style>
