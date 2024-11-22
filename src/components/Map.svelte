<svelte:head>
  <link rel="stylesheet" href="/css/leaflet.css">
</svelte:head>

<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import SubwayLines from './SubwayLines.svelte';
  import SubwayStations from './SubwayStations.svelte';

  let map;
  const dispatch = createEventDispatcher();

  function handleStationclick(event) {
    dispatch('stationClick', event.detail);
  }

  onMount(async () => {
    const L = await import('leaflet');

    map = L.map('map', {
        maxZoom: 20,
        minZoom: 6,
        zoomControl: false
    }).setView([40.7128, -74.0060], 12);

    map.attributionControl.setPrefix('')

    L.control.zoom({
        position: 'bottomright'
    }).addTo(map);

    // temporary dark map before changing back to Stada
    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/">CARTO</a>',
      subdomains: 'abcd',
      maxZoom: 19,
    }).addTo(map);

  });
</script>

<div id="map">
  <SubwayLines {map} />
  <SubwayStations {map} on:stationClick={handleStationclick}/>
</div>

<style>
  #map {
    height: 100vh; 
    width: 100vw;
  }
</style>