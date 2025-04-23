---
title: 'Projects'
date: 2024-05-19
type: landing

design:
  spacing: '5rem'

sections:
  - block: markdown
    content:
      title: Sensor Map
      markdown: |
        <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
        <div id="map" style="height: 600px; width: 100%;"></div>
        <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
        <script>
            var map = L.map('map').setView([39.9042, 116.4074], 13);
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
            }).addTo(map);
            fetch('/data/sensor.geojson')
                .then(response => response.json())
                .then(data => {
                    L.geoJSON(data).addTo(map);
                });
        </script>

  - block: collection
    content:
      title: Selected Projects
      filters:
        folders:
          - project
    design:
      view: article-grid
      columns: 3
---