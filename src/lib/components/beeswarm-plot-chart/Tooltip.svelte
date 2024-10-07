<script lang="ts">
	import { fly, fade } from 'svelte/transition';

	export let data;
	export let colorScale;
	export let width;

	let tooltipWidth: number;

	const xNudge = 5;
	const yNudge = 5;

	$: xPosition =
		data.x + tooltipWidth + xNudge > width ? data.x - tooltipWidth - xNudge : data.x + xNudge;

	$: yPosition = data.y + yNudge;
</script>

<div
	in:fly={{ y: 10, duration: 200, delay: 200 }}
	out:fade
	bind:clientWidth={tooltipWidth}
	class="tooltip"
	style={`top:${yPosition}px; left:${xPosition}px;`}
>
	<h3 class="country">{data.country}</h3>
	<div class="info">
		<span class="score">{data.happiness}/10</span>
		<span class="continent" style={`background:${colorScale(data.continent)}`}
			>{data.continent}</span
		>
	</div>
	<span class="bar background" />
	<span
		class="bar foreground"
		style={`background: ${colorScale(data.continent)}; width: ${data.happiness * 10}%`}
	/>
</div>

<style>
	.tooltip {
		position: absolute;
		background: white;
		box-shadow: 2px 3px 8px rgba(0, 0, 0, 0.15);
		padding: 8px 6px;
		border-radius: 3px;
		pointer-events: none;
		display: flex;
		flex-direction: column;
		gap: 3px;
		transition:
			top 300ms ease,
			left 300ms ease;
	}
	.country {
		font-weight: 600;
		margin-bottom: 4px;
	}
	.info {
		display: flex;
		justify-content: space-between;
		align-items: center;
		gap: 8px;
	}

	.score {
		font-size: 12px;
	}

	.continent {
		font-size: 12px;
		font-weight: 500;
		padding: 3px;
		border-radius: 3px;
	}

	.bar {
		position: absolute;
		bottom: 0;
		left: 0;
		height: 3px;
		width: 100%;
	}

	.bar.background {
		background: #eee;
	}
</style>
