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

    // NDVI
    var ndviBounds = [
      [38.899727, -92.210584],
      [38.898748, -92.209152]
    ];
    var ndviLayer = L.imageOverlay("/images/NDVI.png", ndviBounds, {
      opacity: 1.0
    }).addTo(map);

    // EC-Shallow
    var ecsLayer = L.imageOverlay("/images/ECS.PNG", [
      [38.899755, -92.210554],
      [38.898716, -92.209279]
    ], {opacity: 1.0
    });


    // Yield
    var yieldLayer = L.imageOverlay("/images/yield.png", [
      [38.899703, -92.210552],
      [38.898761, -92.209174]
    ], {opacity: 1.0
    });

    var tnLayer = L.imageOverlay("/images/TN.PNG", [
      [38.899711, -92.210552],
      [38.898761, -92.209172]
    ], {opacity: 1.0
    });

    var socLayer = L.imageOverlay("/images/SOC.PNG", [
      [38.899711, -92.210552],
      [38.898761, -92.209172]
    ], {opacity: 1.0
    });

    var wasLayer = L.imageOverlay("/images/WAS.PNG", [
      [38.899711, -92.210552],
      [38.898761, -92.209172]
    ], {opacity: 1.0
    });

    var phLayer = L.imageOverlay("/images/PH.PNG", [
      [38.898761, -92.209172],  // bottom-left
      [38.899711, -92.210552]   // top-right
    ], {opacity: 1.0
    });

    var kLayer = L.imageOverlay("/images/K.PNG", [
      [38.898761, -92.209172],  // bottom-left
      [38.899711, -92.210552]   // top-right
    ], {opacity: 1.0
    });

    var cecLayer = L.imageOverlay("/images/CEC.PNG", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });

    var pLayer = L.imageOverlay("/images/P.PNG", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });

    // control
    var overlayMaps = {
      "NDVI": ndviLayer,
      "ECa": ecsLayer,
      "Yield": yieldLayer,
      "TN": tnLayer,
      "SOC": socLayer,
      "WAS": wasLayer,
      "pH": phLayer,
      "K": kLayer,
      "P": pLayer,
      "CEC": cecLayer
    };

    L.control.layers(null, overlayMaps).addTo(map);
  });
</script>
