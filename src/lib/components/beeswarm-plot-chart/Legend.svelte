<script>
	export let colorScale;
	const ticks = colorScale.domain();
</script>

<div class="legend">
	{#each ticks as continent}
		<p><span style="background-color: {colorScale(continent)};"></span>{continent}</p>
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
	}
	span {
		width: 10px;
		height: 10px;
		display: inline-block;
		border-radius: 50%;
		margin: 0 3px;
		border: 1px solid rgba(0, 0, 0, 0.5);
	}
</style>
