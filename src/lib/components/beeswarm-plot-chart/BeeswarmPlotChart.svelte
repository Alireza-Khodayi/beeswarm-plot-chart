<script>
	// @ts-nocheck

	import { forceSimulation, forceX, forceY, forceCollide } from 'd3-force';
	import { scaleLinear, scaleBand, scaleOrdinal, scaleSqrt } from 'd3-scale';
	import { mean, rollups } from 'd3-array';
	import AxisX from './AxisX.svelte';
	import AxisY from './AxisY.svelte';
	import Legend from './Legend.svelte';
	import Tooltip from './Tooltip.svelte';

	export let data;

	const RADIUS = 5;
	const simulation = forceSimulation(data); // Create simulation of data

	//Reactive block for simulation to be fully dynamic,physic base and responsive with
	$: {
		simulation
			.force(
				'x',
				forceX()
					.x((d) => xScale(d.happiness))
					.strength(0.8)
			)
			.force(
				'y',
				forceY()
					.y((d) => yScale(d.continent))
					.strength(0.2)
			)
			.force(
				'collide',
				forceCollide().radius((d) => radiusScale(d.happiness))
			)
			.alpha(0.3)
			.alphaDecay(0.0005)
			.restart();
	}

	let width = 400;
	let height = 400;

	const margin = {
		top: 0,
		right: 0,
		bottom: 28,
		left: 0
	};

	$: innerWidth = width - margin.left - margin.right;
	$: innerHeight = height - margin.top - margin.bottom;

	//Generate the average for each continent, so that we can sort acording to that
	const continents = rollups(
		data,
		(v) => mean(v, (d) => d.happiness),
		(d) => d.continent
	) //Group data by continent and return the group-wide mean
		.sort((a, b) => a[1] - b[1]) // Sort acording to value
		.map((d) => d[0]); // Grab the continent name

	$: xScale = scaleLinear()
		.domain([1, 9]) // Input
		.range([0, innerWidth]); // Output
	$: yScale = scaleBand().domain(continents).range([innerHeight, 0]).paddingOuter(0.5);

	let nodes = [];
	simulation.on('tick', () => {
		nodes = simulation.nodes();
	});

	// Add a color to each circle acording to its continent
	const colorRange = ['#dda0dd', '#fe7f2d', '#fcca46', '#a1c181', '#619b8a', '#eae2b7'];
	const colorScale = scaleOrdinal()
		.domain(continents) //Input
		.range(colorRange); // Output

	// Size of circles
	const radiusScale = scaleSqrt()
		.domain([1, 9]) //Input
		.range([3, 8]); //Output

	let hovered;
</script>

<h2 class="chart-title">The Happiest Countries in the World</h2>
<Legend {colorScale} />
<div class="chart-container" bind:clientWidth={width}>
	<svg {width} {height}>
		<AxisX {xScale} width={innerWidth} height={innerHeight} />
		<AxisY {yScale} />
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<g
			class="inner-chart"
			transform={`translate(${margin.left}, ${margin.top})`}
			on:mouseleave={() => {
				hovered = null;
			}}
		>
			{#each nodes as node}
				<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<circle
					cx={node.x}
					cy={node.y}
					r={radiusScale(node.happiness)}
					fill={colorScale(node.continent)}
					stroke="black"
					on:mouseover={() => {
						hovered = node;
					}}
					on:focus={() => {
						hovered = node;
					}}
					tabindex="0"
				/>
			{/each}
		</g>
	</svg>
	{#if hovered}
		<Tooltip data={hovered} />
	{/if}
</div>

<style>
	:global(.tick text, .axis-title) {
		font-weight: 400;
		font-size: 12px;
		fill: #8f8f8f;
	}

	.chart-title {
		font-size: 1.45rem;
		font-weight: 600;
		margin-bottom: 0.5rem;
		text-align: center;
	}
</style>
