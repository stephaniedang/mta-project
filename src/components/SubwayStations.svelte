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

  async function loadArtworkCSV() {
    const data = await d3.csv('/data/clean_art_data.csv');
    return data;
  }

  function countArtworks(stationName, stationRoutes, artworks) {
    const normalizedStationName = stationName.trim().toUpperCase();
    const normalizedStationRoutes = stationRoutes
      .split(/[,\s]+/)
      .map(route => route.trim().toUpperCase());

    const filteredArtworks = artworks.filter(artwork => {
      const artworkStationName = artwork['Station Name'].trim().toUpperCase();
      const artworkRoutes = artwork['Line']
        .split(/[,\s]+/)
        .map(route => route.trim().toUpperCase());

      const hasCommonLines = normalizedStationRoutes.some(route => artworkRoutes.includes(route));

      return artworkStationName === normalizedStationName && hasCommonLines;
    });

    return filteredArtworks.length;
  }

  function getCircleSize(artworkCount) {
    if (artworkCount === 0) return 25;
    return 10 + artworkCount * 15;
  }

  function getCircleStrokeColor(artworkCount) {
    return artworkCount === 0? '#262322' : '#dee2e6';
  }

  function getCircleColor(artworkCount) {
    return artworkCount === 0 ? 'black': 'white';
  }

  onMount(async () => {
    const L = await import('leaflet');
    const stations = await loadStationCSV();
    const artworks = await loadArtworkCSV();

    setTimeout(() => {
      stations.forEach(station => {
        const artworkCount = countArtworks(station.stationName, station.stationRoutes, artworks);

        const circle = L.circle([station.lat, station.lon], {
          radius: getCircleSize(artworkCount),
          color: getCircleStrokeColor(artworkCount),
          fillColor: getCircleColor(artworkCount),
          fillOpacity: 0.8
        }).addTo(map).bindPopup(`<b>${station.stationName}</b>`);

        circle.on('click', () => {
          console.log('Circle Clicked: ', station.stationName);
          console.log('Coordinates: ', station.lat, station.lon)
          dispatch('stationClick', { name: station.stationName, lines: station.stationRoutes }); 
        });
      });
    }, 100);
  });
</script>