---
title: "Projects"
date: 2025-04-22
---

## 项目地图展示（OpenStreetMap + Leaflet）

这是一个地图嵌入测试，中心位置是上海。

<div id="map" style="height: 400px; margin-top: 20px;"></div>

<!-- Leaflet 样式和脚本（使用 jsDelivr 国内可访问） -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css"
/>
<script src="https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    var map = L.map("map").setView([31.2304, 121.4737], 12); // 上海位置

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution: '&copy; OpenStreetMap 贡献者',
    }).addTo(map);

    L.marker([31.2304, 121.4737])
      .addTo(map)
      .bindPopup("你好，这里是上海的一个项目点")
      .openPopup();
  });
</script>
