<script>
  //References (in addition to svelte and d3 documentation):
  //https://dev.to/learners/maps-with-d3-and-svelte-8p3
  //https://svelte.recipes/components/world-map
  //https://bl.ocks.org/rveciana/9026255839233498dbe979ea69ad3af2
  //https://svelte.recipes/components/world-map

  import * as d3 from "d3";
  export let counties = [];
  export let callback;
  export let list = [];
  //   export let legendLabels = [];
  let visibleState = "";

  export const width = 1200,
    height = 550;

  $: projection = d3
    .geoAlbersUsa()
    .translate([width / 2, height / 2])
    .scale([1150]);

  $: path = d3.geoPath(projection);
  //I had issues accessing 'colorScale' from onMount in App.svelte, so I created it manually here.
  $: thisColorScale = d3
    .scaleOrdinal()
    .domain([1, 5])
    .range(["#3b528b", "#fde725", "#440154", "#21908d", "#5dc963"]);

  //create legend
  //const legendColors = ["blue", "yellow", "purple", "teal", "green"];
  const legendColors = d3
    .scaleOrdinal()
    .range(["#3b528b", "#fde725", "#440154", "#21908d", "#5dc963"]);
  const legendLabels = [
    "Low MAT Rate, High Pill Rate, Moderate Death Rate",
    "High MAT Rate, Moderate Pill Rate, High Death Rate",
    "Unknown Risk Status",
    "Low MAT Rate, High Pill Rate, High Death Rate",
    "High MAT Rate, High Pill Rate, High Death Rate",
  ];

  var legendText = legendLabels.map((d) => d);
</script>

//draw map
<g class="paths">
  {#each counties as county}
    <path
      d={path(county)}
      class:selected={list.includes(
        county.properties.NAMELSAD +
          " || " +
          " Deaths per 100,000 [2010-2020]: " +
          county.properties.DEATHSPER +
          " || Pills per 100 [2012]: " +
          county.properties.PILLS +
          " || MAT Providers [2022]: " +
          county.properties.MAT
      )}
      on:click={() =>
        callback(
          county.properties.NAMELSAD +
            " || " +
            " Deaths per 100,000 [2010-2020]: " +
            county.properties.DEATHSPER +
            " || Pills per 100 [2012]: " +
            county.properties.PILLS +
            " || MAT Providers [2022]: " +
            county.properties.MAT
        )}
      on:mouseenter={() => (visibleState = county.properties.GEOID)}
      on:mouseleave={() => (visibleState = "")}
      class="countyShape"
      fill={thisColorScale(county.properties.CL)}
    />
  {/each}
</g>

//show county names on hover
<g class="labels">
  {#each counties as county}
    <text
      class:visible={visibleState === county.properties.GEOID}
      x={path.centroid(county)[0]}
      y={path.centroid(county)[1]}
    >
      {county.properties.NAMELSAD}
    </text>
  {/each}
</g>

//create legend with text labels and rects
<g class="legend">
  {#each legendText as label, i}
    <text class="legend-text" x={60} y={i * 25 + 440}>{label}</text>
  {/each}
</g>
<g>
  {#each legendText as label, i}
    <rect
      class="legend-rect"
      x={20}
      y={i * 24 + 427}
      fill={legendColors(label)}
      width="30"
      height="21"
    />
  {/each}
</g>

<style>
  path {
    stroke: black;
    stroke-width: 1px;
    cursor: pointer;
  }

  path.selected {
    fill: lightblue;
  }

  text {
    opacity: 0;
    /* color: white; */
    pointer-events: none;
    /* text-anchor: middle; */
    transition: opacity 0.5s;
  }

  text.visible {
    opacity: 1;
    fill: #d3d3d3;
  }

  .legend-text {
    opacity: 1;
    color: gray;
    font-size: 12px;
  }
</style>
