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
        });

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
      "CEC": cecLayer,
    };

    L.control.layers(null, overlayMaps).addTo(map);
  });
</script>


## MU Digital Farm
<div id="map2" style="height: 500px; margin-top: 20px;"></div>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    var map2 = L.map("map2").setView([38.911116, -92.263274], 17);

    var satellite2 = L.tileLayer(
      "https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}",
      {
        attribution:
          "Tiles &copy; Esri — Source: Esri, Maxar, Earthstar Geographics, CNES/Airbus DS, USDA, USGS, AeroGRID, IGN, and the GIS User Community",
        maxZoom: 19,
      }
    );
    satellite2.addTo(map2);

    var sensorLayer2;
    fetch("/data/sensor2.geojson")
      .then((response) => response.json())
      .then((geojsonData) => {
        sensorLayer2 = L.geoJSON(geojsonData, {
          onEachFeature: function (feature, layer) {
            let popupContent = "";
            if (feature.properties) {
              popupContent = Object.entries(feature.properties)
                .map(([key, val]) => `<strong>${key}</strong>: ${val}`)
                .join("<br>");
            }
            layer.bindPopup(popupContent || "无属性数据");
          },
        }).addTo(sensorLayer2);

        map2.fitBounds(sensorLayer2.getBounds());
      });

    var ecsLayer2 = L.imageOverlay("/images2/ECS.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0}).addTo(map2);
    
    var TNLayer2 = L.imageOverlay("/images2/TN.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var socLayer2 = L.imageOverlay("/images2/SOC.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var WASLayer2 = L.imageOverlay("/images2/WAS.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var PLayer2 = L.imageOverlay("/images2/P.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var KLayer2 = L.imageOverlay("/images2/K.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var PHLayer2 = L.imageOverlay("/images2/PH.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var overlayMaps2 = {
      "ECa": ecsLayer2,
      "TN": TNLayer2,
      "SOC": socLayer2,
      "WAS": WASLayer2,
      "P": PLayer2,
      "K": KLayer2,
      "pH": PHLayer2,
      "sample": sensorLayer2,
    };

    L.control.layers(null, overlayMaps2).addTo(map2);
  });
</script>
