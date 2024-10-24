<svelte:head>
  <title>Art Off the Rails</title>
  <link rel="stylesheet" href="/css/styles.css">
</svelte:head>

<script>
  import Map from "../components/Map.svelte";
  import Modal from "../components/Modal.svelte";
  import { onMount } from "svelte";
  import { csv } from 'd3-fetch';
  
  let selectedStation = 'all';
  let artworkData = [];

  onMount(() => {
    csv('/data/clean_art_data.csv')
      .then(data => {
        artworkData = data;
      })
      .catch(error => {
        console.error('Error loading CSV:', error);
      });
  });

  function handleStationclick(stationName) {
    selectedStation = stationName;
  }
</script>

{#if artworkData.length > 0}
  <Map />
  <Modal {selectedStation} {artworkData} />
{/if}