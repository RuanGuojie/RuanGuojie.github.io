## 项目地图（OpenStreetMap 测试）

<div id="map" style="height: 400px;"></div>

<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.3/dist/leaflet.css"
  integrity="sha256-sA+e2AtJOCbZo7GkTq7t4J8C8LkhzN4niwHqjvZqs+k="
  crossorigin=""
/>
<script
  src="https://unpkg.com/leaflet@1.9.3/dist/leaflet.js"
  integrity="sha256-nP4cPp2YkJpsZgDdhZYtKPtFfXUfZMtfsDp5Gk6sjRk="
  crossorigin=""
></script>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    var map = L.map("map").setView([31.2304, 121.4737], 13); // 上海位置

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution: '&copy; OpenStreetMap 贡献者',
    }).addTo(map);

    L.marker([31.2304, 121.4737])
      .addTo(map)
      .bindPopup("你好，这里是上海")
      .openPopup();
  });
</script>
