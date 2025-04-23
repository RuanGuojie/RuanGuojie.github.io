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
      text: |
        <style>
            #map { height: 600px !important; width: 100% !important; display: block !important; }
        </style>
        <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
        <div id="map" style="height: 600px; width: 100%;"></div>
        <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
        <script>
            function initializeMap() {
                if (typeof L === 'undefined') {
                    console.error('Leaflet.js not loaded');
                    return;
                }
                try {
                    var map = L.map('map').setView([39.9042, 116.4074], 13);
                    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                        attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
                    }).addTo(map);
                    fetch('/data/sensor.geojson')
                        .then(response => response.json())
                        .then(data => {
                            L.geoJSON(data).addTo(map);
                            map.fitBounds(L.geoJSON(data).getBounds());
                        })
                        .catch(error => console.error('Error loading GeoJSON:', error));
                } catch (error) {
                    console.error('Error initializing map:', error);
                }
            }
            document.addEventListener('DOMContentLoaded', initializeMap);
            setTimeout(initializeMap, 1000);
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