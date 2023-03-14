<script>
  export let chartWidth;
  export let chart;
  export let xScale;
  export let yScale;
  export let barWidths;
  var result = {};
  $: barWidths.forEach((key, i) => result[(i-15)*-1] = barWidths[i]);
  let innerWidth = chartWidth - (chart.left + chart.right);
  let barWidth = innerWidth / 9 // HARD-CODED!!!! 
  console.log(barWidths)
</script>

<g class='bars'  transform="translate(0 2)">
			{#each Object.entries(result) as [tick, barPlace]}
      {#if barPlace != -1} 
				<rect x="{xScale(30)}"
					y="{yScale(tick)}"
					width="{xScale(barPlace)}"
          height="{barWidth - 70}"
				></rect>
{/if}
			{/each}
</g>

<style>

	.bars rect {
		fill: rgb(1, 101, 148);
		stroke: none;
		opacity: 0.65;
	}
</style>


