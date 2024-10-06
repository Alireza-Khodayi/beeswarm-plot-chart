<script lang="ts">
	import { forceSimulation, forceX, forceY, forceCollide } from 'd3-force';
	import { scaleLinear, scaleBand } from 'd3-scale';
	import { mean, rollups } from 'd3-array';
	import type { ICountriesHappiness } from '$lib/interfaces/countries-happiness.interface';
	export let data;

	const RADIUS = 5;
	const simulation = forceSimulation(data);
	$: {
		simulation
			.force(
				'x',
				forceX()
					.x((d: any) => xScale(d.happiness))
					.strength(0.8)
			)
			.force(
				'y',
				forceY()
					.y((d: any): number => yScale(d.continent))
					.strength(0.2)
			)
			.force('collide', forceCollide().radius(RADIUS))
			.alpha(0.3)
			.alphaDecay(0.0005)
			.restart();
	}

	let width = 400;
	let height = 400;

	const margin = {
		top: 0,
		right: 0,
		bottom: 20,
		left: 0
	};

	$: innerWidth = width - margin.left - margin.right;
	$: innerHeight = height - margin.top - margin.bottom;

	const continents = rollups(
		data,
		(v) => mean(v, (d: any) => d.happiness),
		(d: any) => d.continent
	)
		.sort((a: any, b: any) => a[1] - b[1])
		.map((d) => d[0]);

	$: xScale = scaleLinear().domain([1, 9]).range([0, innerWidth]);
	$: yScale = scaleBand().domain(continents).range([innerHeight, 0]).paddingOuter(0.5);

	let nodes: any[] = [];
	simulation.on('tick', () => {
		nodes = simulation.nodes();
	});
</script>

<div class="chart-container" bind:clientWidth={width}>
	<svg {width} {height}>
		<g class="inner-chart" transform={`translate(${margin.left}, ${margin.top})`}>
			{#each nodes as node}
				<circle cx={node.x} cy={node.y} r={RADIUS} fill="steelblue" />
			{/each}
		</g>
	</svg>
</div>
