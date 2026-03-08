## Bradford Research Farm

<div id="map" style="height: 500px; margin-top: 20px; position: relative; z-index: 1;"></div>

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


    // NDVI
    var ndviBounds = [
      [38.899727, -92.210584],
      [38.898748, -92.209152]
    ];
    var ndviLayer = L.imageOverlay("/images/NDVI.png", ndviBounds, {
      opacity: 1.0
    }).addTo(map);

    // EC-Shallow
    var ecsLayer = L.imageOverlay("/images/ECS.png", [
      [38.899755, -92.210554],
      [38.898716, -92.209279]
    ], {opacity: 1.0
    });

    // EC-deep
    var ecdLayer = L.imageOverlay("/images/ECD.png", [
      [38.899755, -92.210554],
      [38.898716, -92.209279]
    ], {opacity: 1.0
    });

    // DEM
    var demLayer = L.imageOverlay("/images/DEM.png", [
      [38.899878, -92.211008],
      [38.898633, -92.208788]
    ], {opacity: 1.0
    });

    // Yield
    var yieldLayer = L.imageOverlay("/images/Yield.png", [
      [38.899703, -92.210552],
      [38.898761, -92.209174]
    ], {opacity: 1.0
    });

    var tnLayer = L.imageOverlay("/images/TN.png", [
      [38.899711, -92.210552],
      [38.898761, -92.209172]
    ], {opacity: 1.0
    });

    var socLayer = L.imageOverlay("/images/SOC.png", [
      [38.899711, -92.210552],
      [38.898761, -92.209172]
    ], {opacity: 1.0
    });

    var wasLayer = L.imageOverlay("/images/WAS.png", [
      [38.899711, -92.210552],
      [38.898761, -92.209172]
    ], {opacity: 1.0
    });

    var phLayer = L.imageOverlay("/images/PH.png", [
      [38.898761, -92.209172],  // bottom-left
      [38.899711, -92.210552]   // top-right
    ], {opacity: 1.0
    });

    var kLayer = L.imageOverlay("/images/K.png", [
      [38.898761, -92.209172],  // bottom-left
      [38.899711, -92.210552]   // top-right
    ], {opacity: 1.0
    });

    var cecLayer = L.imageOverlay("/images/CEC.png", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });

    var pLayer = L.imageOverlay("/images/P.png", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });

    var caLayer = L.imageOverlay("/images/Ca.png", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });

    var mgLayer = L.imageOverlay("/images/Mg.png", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });

    var proteinLayer = L.imageOverlay("/images/Protein.png", [
      [38.899609, -92.210481],
      [38.898819, -92.209280]
    ], {opacity: 1.0
    });

    var respLayer = L.imageOverlay("/images/RESP.png", [
      [38.898761, -92.209172],
      [38.899711, -92.210552]
    ], {opacity: 1.0
    });




    // control
    var overlayMaps = {
      "NDVI": ndviLayer,
      "ECa_S": ecsLayer,
      "ECa_D": ecdLayer,
      "DEM": demLayer,
      "Yield": yieldLayer,
      "Protein": proteinLayer,
      "TN": tnLayer,
      "SOC": socLayer,
      "WAS": wasLayer,
      "pH": phLayer,
      "K": kLayer,
      "P": pLayer,
      "Ca": caLayer,
      "Mg": mgLayer,
      "CEC": cecLayer,
      "Resp": respLayer,
    };

    var layerControl = L.control.layers(null, overlayMaps).addTo(map);

    fetch("/data/sensor.geojson")
      .then((response) => response.json())
      .then((geojsonData) => {

        var sensorLayer = L.geoJSON(geojsonData, {
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

        layerControl.addOverlay(sensorLayer, "Samples");;

      });
  });
</script>


## MU Digital Farm
<div id="map2" style="height: 500px; margin-top: 20px; position: relative; z-index: 1;"></div>

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

    var rgbLayer2 = L.imageOverlay("/images2/RGB.png", [
      [38.912746, -92.267636],
      [38.907721, -92.256181]
    ], {opacity: 1.0}).addTo(map2);

    var ecsLayer2 = L.imageOverlay("/images2/ECS.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});

    var ecdLayer2 = L.imageOverlay("/images2/ECD.png", [
      [38.912300, -92.266332],
      [38.908232, -92.257788]
    ], {opacity: 1.0});
    
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
      "RGB": rgbLayer2,
      "ECa_S": ecsLayer2,
      "ECa_D": ecdLayer2,
      "TN": TNLayer2,
      "SOC": socLayer2,
      "WAS": WASLayer2,
      "P": PLayer2,
      "K": KLayer2,
      "pH": PHLayer2,
    };

    var layerControl2 = L.control.layers(null, overlayMaps2).addTo(map2);

    fetch("/data/sensor2.geojson")
      .then((response) => response.json())
      .then((geojsonData) => {

        var sensorLayer2 = L.geoJSON(geojsonData, {
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

        map2.fitBounds(sensorLayer2.getBounds());

        layerControl2.addOverlay(sensorLayer2, "Samples");;

      });
  });
</script>


## 3D Globe Agriculture

<link href="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Widgets/widgets.css" rel="stylesheet">
<script src="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Cesium.js"></script>

<div id="map3d" style="display: block; width: 100%; height: 600px; margin-top: 20px; border-radius: 4px; z-index: 1; background-color: #1c1c1c;"></div>

<div style="margin-top: 15px; padding: 15px; background: #f8f9fa; border-radius: 5px; border: 1px solid #e9ecef;">
  <strong style="margin-right: 15px;">3D Layers Control:</strong>
  <button onclick="window.loadKmlLayer('/data/NDVI.KMZ')" style="padding: 5px 12px; margin-right: 8px; cursor: pointer; background-color: #007bff; color: white; border: none; border-radius: 3px;">NDVI</button>
  <button onclick="window.clear3DLayers()" style="padding: 5px 12px; color: #dc3545; cursor: pointer;">Clear</button>
</div>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    try {
      // 1. 填入你的 Cesium ion Token
      Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJiNGU2MzgwZS1jNmM0LTQ4MDItOTc1ZS0wMTEyODNmOGNlMTYiLCJpZCI6NDAwMDcwLCJpYXQiOjE3NzI5Mzg2MDJ9.JTTgTyuiRGuJKpLArTT6KoAkzkC4TaB_M_FiOtWPwcU';

      // 2. 初始化 3D 地球 (已更新为适配 1.114 的新版地形 API)
      var viewer = new Cesium.Viewer("map3d", {
        terrain: Cesium.Terrain.fromWorldTerrain(), // 👈 就是这里修改了新语法
        baseLayerPicker: false,
        geocoder: false,
        animation: false,
        timeline: false,
        navigationHelpButton: false
      });

      // 3. 加载 KML 图层的功能
      window.loadKmlLayer = function(kmlUrl) {
        viewer.dataSources.add(Cesium.KmlDataSource.load(kmlUrl, {
          camera: viewer.scene.camera,
          canvas: viewer.scene.canvas,
          clampToGround: true // 尝试贴合地形
        })).then(function(dataSource){
          viewer.flyTo(dataSource);
        }).catch(function(error) {
          alert("图层加载失败: " + kmlUrl + "\n请确认文件路径正确。");
        });
      };

      // 4. 清除图层的功能
      window.clear3DLayers = function() {
        viewer.dataSources.removeAll();
      };

    } catch (error) {
      document.getElementById("map3d").innerHTML = "<div style='padding: 20px; color: red;'><strong>3D地球初始化失败：</strong><br>" + error.message + "</div>";
      console.error("Cesium 初始化详细错误:", error);
    }
  });
</script>