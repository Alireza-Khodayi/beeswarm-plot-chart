<script lang="ts">
	export let colorScale;
	export let hoveredContinent: any;

	const ticks = colorScale.domain();
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="legend"
	on:mouseleave={() => {
		hoveredContinent = null;
	}}
	on:blur={() => {
		hoveredContinent = null;
	}}
>
	{#each ticks as continent}
		<p
			on:mouseover={() => {
				hoveredContinent = continent;
			}}
			on:focus={() => {
				hoveredContinent = continent;
			}}
			class:unhovered={hoveredContinent && hoveredContinent !== continent}
		>
			<span style="background-color: {colorScale(continent)};"></span>{continent}
		</p>
	{/each}
</div>

<style>
	.legend {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		gap: 5px 10px;
		margin-bottom: 5px;
	}
	p {
		font-size: 0.8rem;
		display: flex;
		align-items: center;
		cursor: pointer;
		transition: opacity 300ms ease;
	}
	span {
		width: 10px;
		height: 10px;
		display: inline-block;
		border-radius: 50%;
		margin: 0 3px;
		border: 1px solid rgba(0, 0, 0, 0.5);
	}
	.unhovered {
		opacity: 0.4;
	}
</style>
