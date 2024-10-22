<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  let map;
  let lineColors = {};

  async function loadGeoJSON() {
    const response = await fetch('/data/subwayLines.geojson');
    const geoJsonData = await response.json();
    return geoJsonData;
  }

  async function loadStationCSV() {
    const data = await d3.csv('/data/subwayStations.csv');
    return data.map(d => ({
      stationName: d['Stop Name'],
      stationRoutes: d['Daytime Routes'],
      lat: +d['GTFS Latitude'],
      lon: +d['GTFS Longitude']
    }));
  }

  async function loadLineColors() {
    const response = await fetch('/data/subwayLineColors.json');
    const colors = await response.json();
    return colors;
  }

  onMount(async () => {
    // Dynamically import Leaflet to avoid SSR issues
    const L = await import('leaflet');

    // Initialize the Leaflet map only in the browser
    map = L.map('map').setView([40.7128, -74.0060], 12);

    // Add the base map layer
    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
      attribution: '&copy; <a href="https://carto.com/attributions">CARTO</a>'
    }).addTo(map);

    lineColors = await loadLineColors();

    const subwayLines = await loadGeoJSON();

    L.geoJSON(subwayLines, {
      style: function(feature) {
        const line = feature.properties.rt_symbol;
        const lineColor = lineColors[line];
        return { color: lineColor,
                 weight: 1,
        };
      }
    }).addTo(map);

    const stations = await loadStationCSV();

    // Add circles for the stations
    stations.forEach(station => {
      // const routes = station.stationRoutes.split(' ');
      // const routeColor = routes.find(route => lineColors[route]) || 'blue';
      L.circle([station.lat, station.lon], {
        radius: 5,
        color: 'white',
        fillOpacity: 0.5,
      }).addTo(map).bindPopup(`<b>${station.stationName}</b>`);
    });
  });
</script>

<style>
  @import 'leaflet/dist/leaflet.css';

  #map {
    height: 98vh;
    width: 100%;
  }
</style>

<div id="map">

</div>