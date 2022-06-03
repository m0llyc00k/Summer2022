<script>
  //References (in addition to svelte and d3 documentation):
  //https://dev.to/learners/maps-with-d3-and-svelte-8p3
  //https://svelte.recipes/components/world-map
  //https://bl.ocks.org/rveciana/9026255839233498dbe979ea69ad3af2

  import * as d3 from "d3";
  export let counties = [];
  export let callback;
  export let list = [];
  let visibleState = "";
  //   export let namesExtent = [];
  //   export let colorScale = () => {};

  export const width = 1000,
    height = 550;

  const projection = d3
    .geoAlbersUsa()
    .translate([width / 2, height / 2])
    .scale([1150]);
  const path = d3.geoPath(projection),
    //I had issues accessing 'colorScale' from onMount in App.svelte
    thisColorScale = d3
      .scaleOrdinal()
      .domain([1, 5])
      .range(["#3b528b", "#fde725", "#440154", "#21908d", "#5dc963"]);
</script>

<g class="paths">
  {#each counties as county}
    <path
      d={path(county)}
      class:selected={list.includes(county.properties.GEOID)}
      on:click={() => callback(county.properties.GEOID)}
      on:mouseenter={() => (visibleState = county.properties.GEOID)}
      on:mouseleave={() => (visibleState = "")}
      class="countyShape"
      fill={thisColorScale(county.properties.CL)}
    />
  {/each}
</g>
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
    text-anchor: middle;
    transition: opacity 0.5s;
  }

  text.visible {
    opacity: 1;
    fill: lightgrey;
  }
</style>
