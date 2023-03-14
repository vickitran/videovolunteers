<script>
  import data from "./data/data.js";
  import { range } from "d3-array";
  import { scaleLinear } from "d3-scale";

  import AxisX from "./components/AxisX.svelte";
  import AxisY from "./components/AxisY.svelte";
  import Barchart from "./components/Barchart.svelte";

  let width = 593;
  let height = 700;

  const margin = { top: 20, right: 40, left: 0, bottom: 20 };
  const chart = { top: 20, right: 40, left: 0, bottom: 20 };
  let chartWidth = 800;
  let chartHeight = 300;

  let chartTitle;
  let defaultchartTitle = "Select a district by hovering or clicking!"
  chartTitle = defaultchartTitle
  let dataInfo = null;
  let yLabels = [""];
  let issuesBarWidth = [-1];
  let noissues = null;
  let openissues = null;
  let closeissues = null;
  let notimeissues = null;
  let avgtime = null;
  let peopleaffected = null;
  let peopleaffectedsamplesize = null;
  let affectedgroups = [-1];


import { max , median} from "d3-array";
let maxCategoryValue = 15
let maxValueValue = 80
let maxValue = 220
let medValue = median(data, (d) => d.noissues)
let colorScale = () => {};
    colorScale = scaleLinear()
      .domain([1,medValue,maxValue])
      .range(["orange", "white", "green"])

let ticks = range(1,maxValue,9)
let medTick = median(ticks)

  $: xScale = scaleLinear()
    .domain([0, 120])
    .range([0, chartWidth - chart.left - chart.right]);
  
  const yScale = scaleLinear()
    .domain([0, maxCategoryValue])
    .range([chartHeight - chart.top - chart.bottom, 0]);

</script>
<div style="display:inline-block;vertical-align:top;">
<a href="https://www.videovolunteers.org/" target="_blank"><img src="images/logo.png" alt="videoapp" width="180"></a>
<a href="https://www.vizforsocialgood.com/join-a-project/2023/2/4/video-volunteers-india" target="_blank"><img src="images/vsfg_logo.png" alt="vizforsocialgood" width="180"></a>
</div>
<div style="display:inline-block; margin-top:40px">
<p>Made with 💖 by <a href="https://twitter.com/seven_cakes" target="_blank">Victoria</a></p>
</div>

<div class = "scale-container" style="margin-left:10px"> 
{#each ticks as tick}
		<span style="background:{colorScale(tick)} ; height: 10px; width:20px; display: inline-block;">&nbsp;</span>
{/each}
</div>
<div class = "scale-container"> 
		<p style="display: inline-block;">1</p>
    <p style="display: inline-block;  margin: 0 25.5%;">220</p>
</div>

<div class="row">
  <div class="column">
<div
  class="chart-container"
  bind:clientWidth={width}
>
<svg  {width} {height} viewBox="0 0 815 593" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">

  <g id="Districts_of_Jharkhand_map" transform="translate(1.108506, 1.019941)" fill="#FFFFFF" stroke="#646464" stroke-width="0.98268" stroke-opacity="0.8">
     <!-- svelte-ignore a11y-mouse-events-have-key-events -->
      {#each data as state}
            <!-- svelte-ignore a11y-mouse-events-have-key-events -->
        <polygon
          id={state.name}
          points={state.points}
          fill={colorScale(state.noissues)} 
          opacity={chartTitle ? (chartTitle == state.title ? "1" : ".3") : "1"}
           on:mouseover={() => {
            
            chartTitle = state.title;
            yLabels = state.topissues;
            noissues = state.noissues;
            issuesBarWidth = state.topissuesno;
            openissues = state.open;
            closeissues = state.closed;
            notimeissues = state.timetopublish;
            avgtime=state.timetoimpact;
            peopleaffected = state.peopleaffected;
            peopleaffectedsamplesize = state.peopleaffectedsamplesize;
            affectedgroups = state.affectedgrouplabels;


            
          }}
          on:focus={() => {
            chartTitle = state.title;
            yLabels = state.topissues;
            dataInfo = state;
            noissues = state.noissues;
            issuesBarWidth = state.topissuesno;
            openissues = state.open;
            closeissues = state.closed;
            notimeissues = state.timetopublish;
            avgtime=state.timetoimpact;
            peopleaffected = state.peopleaffected;
            peopleaffectedsamplesize = state.peopleaffectedsamplesize;
            affectedgroups = state.affectedgrouplabels;

          }}
        />

      {/each}
      
  </g>

</svg>
</div>
  </div>
  <div class="column">
    {#if chartTitle}<h1 >{chartTitle}</h1>{/if}
    <div class="row">
      <div class="columnsplit"><div class="numberbox"><p class="header"> Total No. Issues:</p>{#if noissues}<p class="number">{noissues}</p>{:else}<p> --- </p>{/if}</div></div>
      <div class="columnsplit"><div class="numberbox"><p class="header"> Open Issues:</p>{#if openissues}<p class="number">{openissues}</p>{:else}<p> --- </p>{/if}</div></div>
      <div class="columnsplit"><div class="numberbox"><p class="header"> Closed Issues:</p>{#if closeissues}<p class="number">{closeissues}</p>{:else}<p> --- </p>{/if}</div></div>
      <div class="columnsplit"><div class="numberbox"><p class="header"> Avg. Duration(Sample Size: {avgtime})</p> {#if notimeissues}{#if notimeissues == 'not available'}<p style="font-size=90%">{notimeissues}</p>{:else}  <p class="number">{notimeissues} days</p>{/if}
        
    {:else}<p> --- </p>{/if}</div></div>
    </div>

    
    
    <!-- bar chart -->
    <div style="text-align:center; margin-top:50px " bind:clientWidth={width}>
    <h2>Types of Issues in the Community</h2>
    <svg width={chartWidth} height={chartHeight} style="margin-top:20px">

    <AxisX {chartHeight} {xScale} {chart} xTicks={[30,40,50,60,70,80,90,100,110]}/>
    <AxisY {yScale} {chart} {yLabels}/>
    <Barchart {chartWidth} {chart} {xScale} {yScale} barWidths={issuesBarWidth} />
    </svg>
    </div>

    <div style="text-align:center; margin-top:10px; margin-right:100px" bind:clientWidth={width}>
       <div style="float: left; width:60% "> <h3>Groups Affected</h3>
           <svg width={chartWidth} height={chartHeight} style="margin-top: 20px;margin-left:60px">

    <AxisX {chartHeight} {xScale} {chart} xTicks={[30,40,50,60,70]}/>
    <AxisY {yScale} {chart} yLabels={['Children',
 'Scheduled Tribe',
 'Other lower castes (e.g. OBC)',
 'Other religious minorities',
 'The problem is cross-cutting',
 'Scheduled Castes',
 'Women',
 'Tribals',
 'Youth',
 'Transgender',
 'Elderly',
 'Dalits',
 'Adivasi',
 'Muslims',
 '',
 '',
 '']}/>
    <Barchart {chartWidth} {chart} {xScale} {yScale} barWidths={affectedgroups}/>
    </svg></div>
        <div style="float: left; margin-left:50px"><h3>No. Individuals (Sample Size:{peopleaffectedsamplesize})</h3>{#if peopleaffected}<p class="number">{peopleaffected}</p>{:else}<p> --- </p>{/if}</div>
    </div>


  </div>

</div>



<style>
* {
  box-sizing: border-box;
}


/* Create two equal columns that floats next to each other */
.column {
  float: left;
  width: 50%;
  padding: 10px;
  
}
.columnsplit {
  float: left;
  width: 25%;
  
}
h1 {
  
  font-size:300%;
  text-align: center;
  padding-left:50px;
}
h2 {
  font-size:150%;
  text-align: center;
}
h3 {
  font-size:100%;
  text-align: center;
}
/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

/* Responsive layout - makes the two columns stack on top of each other instead of next to each other */
@media screen and (max-width: 600px) {
  .column {
    width: 100%;
  }
}

  polygon {
    transition: r 300ms ease, opacity 300ms ease;
    cursor: pointer;
  }
  .numberbox {
    margin-top: 30px;
    margin-left: 30px;
    margin-right:30px;
    height: 20px;
    width:200px;
    text-align:center;
  }
  .number{
    font-size: 250%;
  }
  .header {
    font-size: 90%;
  }
</style>