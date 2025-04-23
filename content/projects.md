## 项目传感器地图（基于 GeoJSON）

<div id="map" style="height: 500px; margin-top: 20px;"></div>

<!-- Leaflet 样式和脚本 -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css"
/>
<script src="https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    var map = L.map("map").setView([0, 0], 2); // 初始视图

    // 添加地图底图层（OpenStreetMap）
    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution: '&copy; OpenStreetMap 贡献者',
    }).addTo(map);

    // 加载 GeoJSON 数据
    fetch("/data/sensor.geojson")
      .then((response) => response.json())
      .then((geojsonData) => {
        var geoLayer = L.geoJSON(geojsonData, {
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

        // 自动缩放视图到数据边界
        map.fitBounds(geoLayer.getBounds());
      });
  });
</script>
