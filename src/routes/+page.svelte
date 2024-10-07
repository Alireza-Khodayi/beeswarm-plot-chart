<script>
	import data from '$lib/data/data';
	import BeeswarmPlotChart from '$lib/components/beeswarm-plot-chart/BeeswarmPlotChart.svelte';
</script>

<svelte:head>
	<title>The Happiest Countries in the World</title>
</svelte:head>

<div class="container">
	<BeeswarmPlotChart {data} />
</div>

<style>
	.container {
		padding: 5px 10px;
	}
</style>
