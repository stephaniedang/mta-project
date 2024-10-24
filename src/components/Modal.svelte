<script>
  export let selectedStation;
  export let artworkData;

  let stations = [];
  let filteredArtworks = [];

  $: if (artworkData.length > 0) {
    stations = [...new Set(artworkData.map(art => art['Station Name']))];
  }

  $: if (selectedStation && !stations.includes(selectedStation) && selectedStation !== 'all') {
    stations = [selectedStation, ...stations];
  }

  $: if (selectedStation && artworkData.length > 0) {
    updateArtworks();
  }

  function updateArtworks() {
    if (selectedStation === 'all') {
      filteredArtworks = [...artworkData];
    } else {
      filteredArtworks = artworkData.filter(art => {
        const normalizedArtStation = art['Station Name'].trim().toLowerCase();
        const normalizedSelectedStation = selectedStation.trim().toLowerCase();
        return normalizedArtStation === normalizedSelectedStation;
      });
    }
  }
</script>

<div id="artwork-modal" class="modal">
  <div class="modal-content">
    <h2>Art Off the Rails</h2>
    <p>
      Often times, the subway station is a sensory explosion.
      There's a cacophony of noises, a blurring rush of visual
      spectacles, and hopefully minimal suspicious smells. Within
      this dance that millions of New Yorkers two step in everyday,
      there are hidden moments of free (well $2.90 really) art that
      tangos around us. Through the Permanent Art Program, Arts
      for Transit commissions public art across MTA NYC Transit, 
      LIRR, Metro-North, and Staten Island Rail.
    </p>

    <div class="filter-section">
      <label for="station-select"> Artwork in
        <select id="station-select" bind:value={selectedStation}>
          <option value="all">All Stations</option>
          {#each stations as station}
            <option value={station}>{station}</option>
          {/each}
        </select>
      </label>
    </div>

    {#if filteredArtworks.length === 0 && selectedStation !== 'all'}
      <p>There’s no artwork at {selectedStation} Station!</p>
    {:else}
      <ul id="artwork-list">
        {#each filteredArtworks as artwork}
          <li>
            <img src={artwork.direct_image_link} alt="{artwork['Art Title']}" style="width: 100px;">
            <p class="artwork-info">
              <strong>{artwork['Art Title']}</strong> by {artwork.Artist} ({artwork['Art Date']})
              <br />
              {artwork['Art Material']}
            </p>
          </li>
        {/each}
      </ul>
    {/if}
  </div>
</div>

<style>
  .modal {
    position: absolute;
    top: 10px;
    left: 10px;
    width: 375px;
    max-height: 600px;
    padding: 20px;
    background-color: #1b1c1d;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.3);
    z-index: 1000;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    color: white;
  }

  .filter-section {
    display: flex;
    flex-direction: column;
  }

  ul#artwork-list {
    flex-grow: 1;
    max-height: 260px;
    overflow-y: auto;
    padding: 0;
  }

  li {
    list-style-type: none;
    display: flex;
    padding: 6px 0;
  }

  .artwork-info {
    padding: 4px;
  }
</style>