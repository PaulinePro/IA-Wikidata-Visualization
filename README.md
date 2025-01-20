<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Interactive Hackerspace Map</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <style>
        #map {
            height: 600px;
            width: 100%;
        }
        .popup-content img {
            max-width: 200px;
            height: auto;
        }
    </style>
</head>
<body>
    <h1>Interactive Hackerspace Map</h1>
    <div id="map"></div>

    <script>
        // Example data from SPARQL query
        const data = [
            {
                itemLabel: "Hackerspace 1",
                geo: [48.8566, 2.3522],
                countryLabel: "France",
                inception: "2010-01-01",
                website: "https://example.com",
                image: "https://example.com/image1.jpg"
            },
            {
                itemLabel: "Hackerspace 2",
                geo: [40.7128, -74.0060],
                countryLabel: "USA",
                inception: "2015-01-01",
                website: "https://example2.com",
                image: "https://example2.com/image2.jpg"
            }
            // Add more data here...
        ];

        // Initialize the map
        const map = L.map('map').setView([0, 0], 2);

        // Add a base layer
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '© OpenStreetMap contributors'
        }).addTo(map);

        // Function to determine marker color based on inception date
        function getColor(inception) {
            const year = new Date(inception).getFullYear();
            if (year < 2010) return 'red';
            if (year < 2015) return 'orange';
            return 'green';
        }

        // Add markers to the map
        data.forEach(item => {
            const { itemLabel, geo, countryLabel, inception, website, image } = item;
            const color = getColor(inception);

            const marker = L.circleMarker(geo, {
                color: color,
                radius: 8,
                fillOpacity: 0.7
            }).addTo(map);

            const popupContent = `
                <div class="popup-content">
                    <h3>${itemLabel}</h3>
                    <p><strong>Country:</strong> ${countryLabel}</p>
                    <p><strong>Inception:</strong> ${inception}</p>
                    <p><a href="${website}" target="_blank">Website</a></p>
                    <img src="${image}" alt="${itemLabel}">
                </div>
            `;
            marker.bindPopup(popupContent);
        });
    </script>
</body>
</html>
