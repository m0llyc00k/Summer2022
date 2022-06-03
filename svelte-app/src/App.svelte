<script>
  import Map from "./Map.svelte";
  import List from "./List.svelte";
  import { onMount } from "svelte";
  import * as d3 from "d3";

  const mapURL =
    "https://raw.githubusercontent.com/m0llyc00k/Summer2022/main/counties.json";

  const width = 1000,
    height = 550;

  let counties = [];
  let namesExtent = [];
  let colorScale = () => {};
  let list = [];

  //get data
  onMount(async () => {
    await d3.json(mapURL).then((geojson) => {
      counties = geojson.features;
      console.log(counties);

      //could not access this outside of onMount function despite trying to 'export' it.
      namesExtent = d3.extent(counties, (d) => d.properties.CL);
      colorScale = d3
        .scaleOrdinal()
        .domain([1, 5])
        .range(["#3b528b", "#fde725", "#440154", "#21908d", "#5dc963"]);
      thisColor = colorScale(counties[50].properties.CL);

      console.log("extent:" + namesExtent + " / " + "color:" + thisColor);
    });
  });

  function onToggleState(county) {
    const countyIndex = list.indexOf(county);
    if (countyIndex === -1) {
      list = [county];
    } else {
      list = [];
    }
  }
</script>

<main class="main-map">
  <h1>Mapping Vulnerability in The Opioid Crisis</h1>
  <svg {width} {height}>
    <Map {counties} callback={onToggleState} {list} />
  </svg>
  <List {list} />
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    /* text-transform: uppercase; */
    font-size: 3em;
    font-weight: 100;
    padding-top: 0;
    margin-top: 0;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
