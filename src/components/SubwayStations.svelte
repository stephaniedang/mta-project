<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  export let map;

  async function loadStationCSV() {
    const data = await d3.csv('/data/subwayStations.csv');
    return data.map(d => ({
      stationName: d['Stop Name'],
      stationRoutes: d['Daytime Routes'],
      lat: +d['GTFS Latitude'],
      lon: +d['GTFS Longitude']
    }));
  }

  onMount(async () => {
    const L = await import('leaflet');
    const stations = await loadStationCSV();

    // Delay rendering of stations to ensure lines are added first
    setTimeout(() => {
      stations.forEach(station => {
        L.circle([station.lat, station.lon], {
          radius: 50,
          color: 'white',
          fillOpacity: 0.5
        }).addTo(map).bindPopup(`<b>${station.stationName}</b>`);
      });
    }, 50);
  });
</script>