<svelte:head>
  <link rel="stylesheet" href="/css/modal.css">
</svelte:head>

<script> 
  export let selectedStation;
  export let selectedLines = [];
  export let selectedIdentifier = '';
  export let artworkData;

  let stations = [];
  let filteredArtworks = [];

  $: if (artworkData.length > 0) {
    stations = [...new Set(artworkData.map(art => art['Station Name']))];
  }

  $: if (selectedIdentifier || selectedStation === 'all' && artworkData.length > 0) {
    updateArtworks();
  }

  function updateArtworks() {
    if (selectedStation === 'all') {
      filteredArtworks = [...artworkData];
    } else {
      filteredArtworks = artworkData.filter(art => {
        const normalizedArtStation = art['Station Name'].trim().toLowerCase();
        const normalizedSelectedStation = selectedStation.trim().toLowerCase();
        const artLines = art['Line'].split(',').map(line => line.trim());
        return (
          normalizedArtStation === normalizedSelectedStation &&
          selectedLines.some(line => artLines.includes(line))
        );
      });
    }
  }
</script>

<div id="artwork-modal" class="modal">
  <div class="modal-content">
    <h2 class="header-title">Art Off the Rails</h2>
    <p class="intro-text">
      Often times, the subway station is a sensory overload.
      There's a caterwauling of noises, a blur of visual
      spectacles, and occasionally <i>suspicious</i> smells. Within
      this dance that millions of New Yorkers two step in everyday,
      there are hidden moments of free (well $2.90 really) art that
      twirls around us. Through the Permanent Art Program, Arts
      for Transit commissions public art across MTA NYC Transit and Staten Island Rail. We pass by so much
      art everyday without even realizing it. How many of these do 
      you have you encountered on your commutes before? Click on an image 
      to read their description page and learn more!
    </p>

    <div class="legend-section">
      <h3 class="legend-title">Legend</h3>
      <div class="legend-item">
        <span class="legend-dot white-dot"></span>
        <span class="legend-description">Station with artworks</span>
      </div>
      <div class="legend-item">
        <span class="legend-dot black-dot"></span>
        <span class="legend-description">Station without artworks</span>
      </div>
      <div class="legend-item">
        <span class="legend-circle-example"></span>
        <span class="legend-description">Larger circles = more artworks</span>
      </div>
    </div>

    <div class="filter-section">
      <label for="station-select"> 
        <span class="label-text">Artwork at</span>
        <select id="station-select" bind:value={selectedStation}>
          <option value="all">All Stations</option>
          {#each stations as station}
            <option value={station}>{station}</option>
          {/each}
        </select>
      </label>
    </div>

    {#if filteredArtworks.length === 0 && selectedStation !== 'all'}
      <p class="no-art-message">🎨 Oh no! There’s no artwork at {selectedStation} Station.</p>
    {:else}
      <ul id="artwork-list">
        {#each filteredArtworks as artwork}
          <li class="art-item">
            <a href="{artwork['Art Image Link']}" target="_blank" rel="noopener noreferrer"><img src={artwork.direct_image_link} alt="{artwork['Art Title']}" class="art-image"></a>
            <p class="artwork-info">
              <span class="title-label">{artwork['Art Title']}</span>
              <br />
              <span class="station-label">📍 {artwork['Station Name']}</span>
              <br />
              <span class="artist-label">⭐️ <i>{artwork.Artist} ({artwork['Art Date']})</i></span>
              <br />
              <span class="material-label">🎨 {artwork['Art Material']}</span>
            </p>
          </li>
        {/each}
      </ul>
    {/if}
  </div>
</div>