<script>
	export let data;
</script>

<div class="tooltip" style={`top:${data.y}px; left:${data.x}px;`}>{data.country}</div>

<style>
	.tooltip {
		position: absolute;
	}
</style>
