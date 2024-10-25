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
  let selectedLines = [];
  let selectedIdentifier = '';
  let artworkData = [];

  onMount(() => {
    csv('/data/clean_art_data.csv')
      .then(data => {
        artworkData = data;
        console.log('Artwork Data Loaded: ', artworkData)
      })
      .catch(error => {
        console.error('Error loading CSV:', error);
      });
  });

  function handleStationclick(event) {
    const { name, lines } = event.detail;
    selectedStation = name;
    selectedLines = lines.split(/\s+/);
    selectedIdentifier = `${name}-${lines}`;
    console.log('Station Clicked: ', name);
    console.log('selectedIdentifier: ', selectedIdentifier);
    console.log('selectedLines:', selectedLines);
  }
</script>

{#if artworkData.length > 0}
  <Map on:stationClick={handleStationclick}/>
  <Modal {selectedStation} {selectedLines} {selectedIdentifier} {artworkData} />
{/if}