<script>
  export let chartHeight;
  export let xScale;
  export let chart;
  export let xTicks;
</script>

<g class = 'tick'>
  {#each xTicks as tick}
  <text
      x={xScale(tick-1)}
      y={chartHeight - chart.bottom}
      dy="6"
      dominant-baseline="hanging"
      >{tick-30}</text
    >
    <line
      x1={xScale(tick)}
      y1="0"
      x2={xScale(tick)}
      y2={chartHeight - chart.top}
      stroke="#cecece"
    />
    {/each}
</g>

<style>
  	.tick line {
		stroke: #e2e2e2;
		stroke-dasharray: 2;
	}
  	.tick text {
		fill: #545454;
    font-size:75%;
    
	}
  </style>