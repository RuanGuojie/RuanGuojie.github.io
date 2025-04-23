## Bradford Research Farm

<div id="map" style="height: 500px; margin-top: 20px;"></div>

<!-- Leaflet 样式和脚本 -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css"
/>
<script src="https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    var map = L.map("map").setView([38.8996, -92.2104], 17); // 大致初始位置

    // 卫星底图
    var satellite = L.tileLayer(
      "https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}",
      {
        attribution:
          "Tiles &copy; Esri — Source: Esri, Maxar, Earthstar Geographics, CNES/Airbus DS, USDA, USGS, AeroGRID, IGN, and the GIS User Community",
        maxZoom: 19,
      }
    );

    satellite.addTo(map);

    // 加载 GeoJSON 数据
    var sensorLayer;
    fetch("/data/sensor.geojson")
      .then((response) => response.json())
      .then((geojsonData) => {
        sensorLayer = L.geoJSON(geojsonData, {
          onEachFeature: function (feature, layer) {
            let popupContent = "";

            if (feature.properties) {
              popupContent = Object.entries(feature.properties)
                .map(([key, val]) => `<strong>${key}</strong>: ${val}`)
                .join("<br>");
            }

            layer.bindPopup(popupContent || "无属性数据");
          },
        }).addTo(map);

        map.fitBounds(sensorLayer.getBounds());
      });

    // NDVI 图层（PNG + World File）
    var ndviBounds = [
      [38.8995872099, -92.2105835951],
      [38.8997272099, -92.2103788951]
    ];
    var ndviLayer = L.imageOverlay("/images/NDVI.png", ndviBounds, {
      opacity: 0.5
    }).addTo(map);

    // 图层控制器（可选开关）
    var overlayMaps = {
      "NDVI 图层": ndviLayer
    };

    L.control.layers(null, overlayMaps).addTo(map);
  });
</script>
