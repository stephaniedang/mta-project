<script>
  import { onMount } from 'svelte';
  export let map;

  let lineColors = {};

  async function loadLineColors() {
    const response = await fetch('/data/subwayLineColors.json');
    const colors = await response.json();
    return colors;
  }

  async function loadGeoJSON() {
    const response = await fetch('/data/subwayLines.geojson');
    const geoJsonData = await response.json();
    return geoJsonData;
  }

  onMount(async () => {
    const L = await import('leaflet');
    lineColors = await loadLineColors();

    const subwayLines = await loadGeoJSON();

    L.geoJSON(subwayLines, {
      style: function (feature) {
        const line = feature.properties.rt_symbol;
        const lineColor = lineColors[line] || '#000';
        return { color: lineColor, weight: 5 };
      }
    }).addTo(map); 
  });
</script>
