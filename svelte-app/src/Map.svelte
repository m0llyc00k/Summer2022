<script>
  //References (in addition to svelte and d3 documentation):
  //https://dev.to/learners/maps-with-d3-and-svelte-8p3

  import * as d3 from "d3";
  //   import { draw } from "svelte/transition";
  //   import { quadInOut } from "svelte/easing";
  export let counties = [];
  export const width = 1200,
    height = 600;

  //   const mapWidth = 1200,
  //     mapHeight = 600;

  const projection = d3
    .geoAlbersUsa()
    .translate([width / 2, height / 2])
    .scale([1300]);
  const path = d3.geoPath(projection);

  //   const namesExtent = d3.extent(counties, (d) => d.properties.class.length);
  //   console.log(namesExtent);
  //   const colorScale = d3
  //     .scaleLinear()
  //     .domain(namesExtent)
  //     .range(["#feedde", "#fd8d3c"]);
</script>

{#each counties as county}
  <path
    d={path(county)}
    class="countyShape"
    fill={colorScale(county.properties.class)}
  />
{/each}

<style>
  path {
    /* fill: white; */
    stroke: gray;
    stroke-width: 1px;
    /* cursor: pointer; */
  }
  /* path.selected {
    fill: lightblue;
  }
  text {
    opacity: 0;
    pointer-events: none;
    text-anchor: middle;
    transition: opacity 0.5s;
  }
  text.visible {
    opacity: 1;
  } */
</style>
