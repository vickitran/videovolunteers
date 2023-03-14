<script>
  export let yScale;
  export let chart;
  export let yLabels;
  var result = {};
  $: yLabels.forEach((key, i) => result[(i-15)*-1] = yLabels[i]);
  console.log(result)
</script>

<g class='tick' transform="translate({chart.left} {chart.top})">
  {#each Object.entries(result) as [tick, yLabel]}<text x="0" y={yScale(tick)} dy="-6"
      >{yLabel}</text
    >
{/each}
</g>

<style>

  	.tick text {
		fill: #545454;;
		text-anchor: start;
    font-size:80%;
	}
  </style>