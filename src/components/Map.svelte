<svelte:head>
  <link rel="stylesheet" href="/css/leaflet.css">
</svelte:head>

<script>
  import { onMount } from 'svelte';
  import SubwayLines from './SubwayLines.svelte';
  import SubwayStations from './SubwayStations.svelte';

  let map;

  onMount(async () => {
    const L = await import('leaflet');

    // Initialize the Leaflet map
    map = L.map('map').setView([40.7128, -74.0060], 12);

    // Add the base map layer
    L.tileLayer('https://tiles.stadiamaps.com/tiles/alidade_smooth_dark/{z}/{x}/{y}{r}.png', {
      attribution: '&copy; <a href="https://www.stadiamaps.com/" target="_blank">Stadia Maps</a> &copy; <a href="https://openmaptiles.org/" target="_blank">OpenMapTiles</a> &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);
  });
</script>

<div id="map">
  <SubwayLines {map} />
  <SubwayStations {map} />
</div>

<style>
  #map {
    height: 100vh; 
    width: 100vw;
  }
</style>