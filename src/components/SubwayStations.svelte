<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import * as d3 from 'd3';
  export let map;

  const dispatch = createEventDispatcher();

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

    setTimeout(() => {
      stations.forEach(station => {
        const circle = L.circle([station.lat, station.lon], {
          radius: 30,
          color: 'white'
        }).addTo(map).bindPopup(`<b>${station.stationName}</b>`);

        circle.on('click', () => {
          console.log('Circle Clicked: ', station.stationName);
          dispatch('stationClick', station.stationName); 
        });
      });
    }, 100);
  });
</script>