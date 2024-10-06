<script>
	export let yScale;

	const ticks = yScale.domain();
</script>

<g class="axis y">
	{#each ticks as tick}
		<g class="tick">
			<text y={yScale(tick)} x={0}>{tick}</text>
		</g>
	{/each}
</g>
