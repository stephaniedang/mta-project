# NYC Subway Art Visualization

### Project Overview
This interactive visualization provides an immersive look at public artwork across NYC subway stations. Through an intuitive map interface, users can explore each station's artworks, visualized as circles on the map. Each station’s circle size and color indicate the presence and number of artworks, while a pop-up modal displays detailed information about individual pieces, artist backgrounds, and links to further descriptions.

---

## Features

- **Map with Interactive Circles**: Each station is represented by a circle, with the size indicating the count of artworks and color denoting the presence of art.
- **Artwork Modal**: Clicking a circle will update the list all artworks available at the station. Users can read high-level artwork information, view images, and find links to external descriptions.
- **Dropdown Station Filter**: The dropdown allows users to browse artworks across all stations or select a specific station for a focused view.
- **Dynamic Data Filtering**: The modal updates dynamically to show relevant artworks based on the selected station, ensuring accurate results.

---

## Project Structure

- **Data Sources**:
  - `Subway Lines:` https://data.cityofnewyork.us/Transportation/Subway-Lines/3qz8-muuu
  - `Subway Stations:` https://catalog.data.gov/dataset/mta-subway-stations
  - `MTA Permanent Art Catalog:` https://data.ny.gov/Transportation/MTA-Permanent-Art-Catalog-Beginning-1980/4y8j-9pkd/about_data 
- **Data Files**:
  - `subwayStations.csv`: Contains subway station data with coordinates and route information.
  - `clean_art_data.csv`: Contains artwork data with station names, lines, artist information, and image links.
  - `subwayLineColors.json`: Contains corresponding colors for each MTA subway line.
  - `subwayLines.geojson`: Contains MTA subway lines and corresponding latitude and longitude information.
- **Components**:
  - `Map.svelte`: Manages the map and station circles, including size and color scaling.
  - `Modal.svelte`: Displays filtered artwork data and allows interaction with each artwork.
  - `SubwayStations.svelte`: Renders circles on the map for each station based on station coordinates and the count of artworks. Passes interaction data to `Map.svelte` for display in the modal.
  - `SubwayLines.svelte`: Manages the display of subway lines with color coding based on line routes, ensuring accurate and visually appealing line representation on the map.

---

## Usage Instructions

- **Explore Stations**: Click on any station circle on the map to view its art pieces.
- **View Art Details**: In the modal, click on artwork images or titles for more information.
- **Filter by Station**: Use the dropdown to view artworks across all stations or a specific one.

---

## Project Details and Approach

- **Data Handling**: Artwork data is dynamically loaded and filtered based on selected station and subway line.
- **Dynamic Filtering Logic**: Artwork filtering in `Modal.svelte` matches station names and lines, ensuring the correct artworks display for each station.
- **Map Layering**: `SubwayStations.svelte` handles station circles, `SubwayLines.svelte` manages line display, and `Map.svelte` combines these for a cohesive map layout.
- **Customization**: Adjust the styles in `modal.css` and `styles.css` to personalize appearance.

---

## Future Improvements

- **Additional Interactivity**: Add hover effects for stations to enhance exploration experience.
- **Search by Artist**: Integrate an artist search to allow users to find artworks by specific creators.
- **Route Filtering**: Enable filtering artworks by subway line to give users a themed journey across stations.
- **Ridership Analytics**: Add additional visual queues or text that shows average ridership for each line/station. How many people potentially pass by these artworks per day/week/month?

---

## Inspiration

- https://jpwright.github.io/subway/
- https://www.theweekendest.com/trains 

---

## License
This project is open-source and available under the MIT License.