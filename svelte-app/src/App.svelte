<script>
  import Map from "./Map.svelte";
  import { onMount } from "svelte";
  import * as d3 from "d3";

  const mapURL =
    "https://raw.githubusercontent.com/m0llyc00k/Summer2022/main/counties.json";
  const width = 1200,
    height = 700;

  let counties = [];

  onMount(async () => {
    await d3.json(mapURL).then((geojson) => {
      counties = geojson.features;
      console.log(counties);
    });
  });
</script>

<main class="main-map">
  <h1>Mapping Vulnerability in The Opioid Crisis</h1>
  <svg {width} {height}>
    <Map {counties} />
  </svg>
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
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
