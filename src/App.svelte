<script lang="ts">
	let defaultFontSize: number = 16;
	let minViewportWidth: number = 320;
	let maxViewportWidth: number = 1280;
	let minFontSize: number = 1;
	let maxFontSize: number = 2;
	let currentViewportWidth: number;

	$: minViewportWidthInRem = minViewportWidth / defaultFontSize;
	$: maxViewportWidthInRem = maxViewportWidth / defaultFontSize;

	$: slope = (maxFontSize - minFontSize) / (maxViewportWidthInRem - minViewportWidthInRem);
	$: yAxisIntersection = -minViewportWidthInRem * slope + minFontSize;

	$: clampString = `font-size: clamp(${minFontSize}rem, ${yAxisIntersection.toFixed(4)}rem + ${(
		slope * 100
	).toFixed(4)}vw, ${maxFontSize}rem);`;

	function copyClampString() {
		navigator.clipboard.writeText(clampString);
	}
</script>

<svelte:window bind:innerWidth={currentViewportWidth} />

<header>
	<h1>Font Clamper</h1>
	<p>A tool to linearly scale font size using CSS clamp.</p>
	<p>
		Inspired by <a
			href="https://css-tricks.com/linearly-scale-font-size-with-css-clamp-based-on-the-viewport/"
			>Pedro Rodriguez' article on CSS-TRICKS</a
		>.
	</p>
</header>

<main>
	<section class="calculator">
		<a class="calculator__help" href="#howto">How to use the Font Clamper</a>
		<div class="inputs flex column">
			<div class="input__row flex">
				<label for="defaultFontSize">Default font size in pixels: </label>
				<input id="defaultFontSize" type="number" min="1" bind:value={defaultFontSize} />
			</div>
			<div class="input__row flex">
				<label for="minViewportWidth">Minimum viewport width in pixels: </label>
				<input id="minViewportWidth" type="number" min="1" bind:value={minViewportWidth} />
			</div>
			<div class="input__row flex">
				<label for="maxViewportWidth">Maximum viewport width in pixels: </label>
				<input id="maxViewportWidth" type="number" min="1" bind:value={maxViewportWidth} />
			</div>
			<div class="input__row flex">
				<label for="minFontSize">Minimum font size in rem: </label>
				<input id="minFontSize" type="number" min="1" bind:value={minFontSize} />
			</div>
			<div class="input__row flex">
				<label for="maxFontSize">Maximum font size in rem: </label>
				<input id="maxFontSize" type="number" min="1" bind:value={maxFontSize} />
			</div>
		</div>
	</section>

	<section class="output">
		<div class="output__wrapper">
			<code class="output__string">{clampString}</code>
			<button class="output__button" on:click={copyClampString}>Copy to clipboard</button>
		</div>
	</section>

	<section class="example">
		<p class="example__text" style={`font-size: ${clampString}`}>
			The <code>font-size</code> of this paragraph will scale linearly from
			<code>{minFontSize}rem</code>
			at a viewport width of <code>{minViewportWidth}px</code> or smaller to
			<code>{maxFontSize}rem</code>
			at a viewport width of <code>{maxViewportWidth}px</code> or larger. Resize the window to see the
			effect in action.
		</p>
		<p class="viewport-width">Current viewport width: {currentViewportWidth}px</p>
	</section>

	<section class="howto">
		<a href="#howto">
			<h2 id="howto">How to use Font Clamper</h2>
		</a>
		<ol class="howto__steps">
			<li class="howto__step">
				<h3>Enter the default font size</h3>
				<p>
					This sets how many pixels one rem equals. For most browsers it's 16px.
					<strong
						>Note that the linear scaling won't work properly for users with a different font size
						than the one you use for the calculation.
					</strong>
				</p>
			</li>
			<li class="howto__step">
				<h3>Enter the min and max viewport widths</h3>
				<p>These values will be used as breakpoints.</p>
			</li>
			<li class="howto__step">
				<h3>Enter the min and max font sizes</h3>
				<p>
					The minimum font size will be used at viewports at (and narrower than) the minimum
					viewport width, and the maximum font size will be used at viewports at (and wider than)
					the maximum viewport width. The font size will scale linearly between the minimum and
					maximum font sizes.
				</p>
			</li>
			<li class="howto__step">
				<h3>Copy the output and paste it in your CSS file</h3>
			</li>
		</ol>
	</section>

	<section class="more">
		<h2>Learn more</h2>
		<p class="more__text">
			Read Pedro's article
			<a
				href="https://css-tricks.com/linearly-scale-font-size-with-css-clamp-based-on-the-viewport/"
				>Linearly Scale font-size with CSS clamp() Based on the Viewport</a
			>
		</p>
		<p class="more__text">
			Read about clamp on
			<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/clamp">MDN</a>
		</p>
	</section>
</main>

<footer>
	<a
		class="link"
		href="https://github.com/aaxelr"
		target="_blank"
		rel="noreferrer"
		aria-label="GitHub"
	>
		aaxelr
		<svg
			xmlns="http://www.w3.org/2000/svg"
			width="1em"
			height="1em"
			viewBox="0 0 15 15"
			aria-hidden="true"
			><path
				fill="currentColor"
				fill-rule="evenodd"
				d="M7.5.25a7.25 7.25 0 0 0-2.292 14.13c.363.066.495-.158.495-.35c0-.172-.006-.628-.01-1.233c-2.016.438-2.442-.972-2.442-.972c-.33-.838-.805-1.06-.805-1.06c-.658-.45.05-.441.05-.441c.728.051 1.11.747 1.11.747c.647 1.108 1.697.788 2.11.602c.066-.468.254-.788.46-.969c-1.61-.183-3.302-.805-3.302-3.583a2.8 2.8 0 0 1 .747-1.945c-.075-.184-.324-.92.07-1.92c0 0 .61-.194 1.994.744A6.963 6.963 0 0 1 7.5 3.756A6.97 6.97 0 0 1 9.315 4c1.384-.938 1.992-.743 1.992-.743c.396.998.147 1.735.072 1.919c.465.507.745 1.153.745 1.945c0 2.785-1.695 3.398-3.31 3.577c.26.224.492.667.492 1.343c0 .97-.009 1.751-.009 1.989c0 .194.131.42.499.349A7.25 7.25 0 0 0 7.499.25Z"
				clip-rule="evenodd"
			/></svg
		>
	</a>
</footer>

<style>
	.flex {
		display: flex;
	}

	.column {
		flex-direction: column;
	}

	.inputs {
		margin: 2rem auto 0;
		max-width: 500px;
	}

	.input__row {
		gap: 1rem;
		justify-content: space-between;
		margin-bottom: 1rem;
	}

	.output__wrapper {
		margin: 1rem 0;
		padding: 1rem;
		border: 1px solid rgba(255, 255, 255, 0.87);
		border-radius: 5px;
	}

	.output__string {
		font-size: 1.25rem;
	}

	.viewport-width {
		font-size: clamp(1.25rem, 1.1667rem + 0.4167vw, 1.5rem);
		font-weight: bold;
	}

	.example__text {
		margin: 3rem;
	}

	.howto,
	.more {
		max-width: 600px;
		margin: 4rem auto;
	}

	.howto__steps {
		text-align: left;
		padding: 0;
	}

	.howto__step {
		font-size: clamp(1.5rem, 1.3333rem + 0.8333vw, 2rem);
	}

	.more__text {
		text-align: left;
	}

	.link {
		font-size: clamp(1.3125rem, 1.25rem + 0.3125vw, 1.5rem);
	}

	@media (prefers-color-scheme: light) {
		.output__wrapper {
			border-color: #213547;
		}
	}
</style>
