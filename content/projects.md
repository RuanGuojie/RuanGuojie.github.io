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
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,500;0,9..40,700;1,9..40,400&display=swap');

.globe-wrapper * {
  box-sizing: border-box;
  font-family: 'DM Sans', sans-serif;
}

.globe-wrapper {
  position: relative;
  z-index: 1;
}

.globe-wrapper #map3d {
  display: block;
  width: 100%;
  height: 600px;
  margin-top: 20px;
  border-radius: 12px;
  position: relative;
  z-index: 1;
  background-color: #0a0e17;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0,0,0,0.3);
}

.layer-control {
  position: relative;
  z-index: 2;
  margin-top: 16px;
  padding: 20px 24px;
  border-radius: 14px;
  border: 1px solid rgba(128,128,128,0.2);
}

.layer-control-title {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 16px;
  font-size: 13px;
  font-weight: 500;
  opacity: 0.45;
  text-transform: uppercase;
  letter-spacing: 1.5px;
}

.layer-control-title::before {
  content: '';
  display: inline-block;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #3b82f6;
  box-shadow: 0 0 8px rgba(59,130,246,0.4);
}

.layer-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.lyr-btn {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 18px;
  border: 1px solid rgba(128,128,128,0.25);
  border-radius: 10px;
  cursor: pointer;
  background: transparent;
  font-size: 14px;
  font-weight: 500;
  font-family: 'DM Sans', sans-serif;
  color: inherit;
  opacity: 0.55;
  -webkit-appearance: none;
  appearance: none;
  touch-action: manipulation;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.lyr-btn:hover {
  opacity: 0.8;
  border-color: rgba(128,128,128,0.4);
}

.lyr-btn.active {
  opacity: 1;
  border-color: rgba(59,130,246,0.5);
  color: #3b82f6;
  background: rgba(59,130,246,0.08);
  box-shadow: 0 0 16px rgba(59,130,246,0.08);
}

.lyr-btn.active .lyr-dot {
  background: #3b82f6;
  box-shadow: 0 0 8px rgba(59,130,246,0.5);
  border-color: transparent;
}

.lyr-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: 1.5px solid rgba(128,128,128,0.4);
  background: transparent;
  flex-shrink: 0;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.fs-btn {
  position: absolute;
  bottom: 36px;
  right: 16px;
  z-index: 2;
  width: 40px;
  height: 40px;
  border-radius: 10px;
  border: 1px solid rgba(255,255,255,0.1);
  background: rgba(0,0,0,0.5);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255,255,255,0.6);
  transition: all 0.2s ease;
  touch-action: manipulation;
  -webkit-appearance: none;
  appearance: none;
  padding: 0;
}

.fs-btn:hover {
  background: rgba(0,0,0,0.7);
  color: #fff;
  border-color: rgba(255,255,255,0.2);
}

.fs-btn svg {
  width: 18px;
  height: 18px;
}

/* Shared fullscreen layer styles */
.globe-wrapper:-webkit-full-screen .layer-control,
.globe-wrapper:fullscreen .layer-control,
.globe-wrapper.fake-fs .layer-control {
  position: fixed;
  bottom: 12px;
  left: 50%;
  transform: translateX(-50%);
  margin: 0;
  z-index: 999999;
  backdrop-filter: blur(24px) saturate(1.4);
  -webkit-backdrop-filter: blur(24px) saturate(1.4);
  background: rgba(255, 255, 255, 0.18);
  border: 1px solid rgba(255,255,255,0.25);
  box-shadow: 0 8px 32px rgba(0,0,0,0.3), inset 0 1px 0 rgba(255,255,255,0.2);
  color: #fff;
  padding: 14px 18px;
  max-height: 35vh;
  display: flex;
  flex-direction: column;
}

.globe-wrapper:-webkit-full-screen .layer-control .layer-control-title,
.globe-wrapper:fullscreen .layer-control .layer-control-title,
.globe-wrapper.fake-fs .layer-control .layer-control-title {
  color: rgba(255,255,255,0.5);
  opacity: 1;
  margin-bottom: 10px;
  font-size: 11px;
  flex-shrink: 0;
}

.globe-wrapper:-webkit-full-screen .layer-control .layer-grid,
.globe-wrapper:fullscreen .layer-control .layer-grid,
.globe-wrapper.fake-fs .layer-control .layer-grid {
  gap: 8px;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  flex: 1;
  min-height: 0;
}

/* Thin scrollbar */
.globe-wrapper:-webkit-full-screen .layer-control .layer-grid::-webkit-scrollbar,
.globe-wrapper:fullscreen .layer-control .layer-grid::-webkit-scrollbar,
.globe-wrapper.fake-fs .layer-control .layer-grid::-webkit-scrollbar {
  width: 4px;
}

.globe-wrapper:-webkit-full-screen .layer-control .layer-grid::-webkit-scrollbar-track,
.globe-wrapper:fullscreen .layer-control .layer-grid::-webkit-scrollbar-track,
.globe-wrapper.fake-fs .layer-control .layer-grid::-webkit-scrollbar-track {
  background: transparent;
}

.globe-wrapper:-webkit-full-screen .layer-control .layer-grid::-webkit-scrollbar-thumb,
.globe-wrapper:fullscreen .layer-control .layer-grid::-webkit-scrollbar-thumb,
.globe-wrapper.fake-fs .layer-control .layer-grid::-webkit-scrollbar-thumb {
  background: rgba(255,255,255,0.2);
  border-radius: 2px;
}

.globe-wrapper:-webkit-full-screen .layer-control .lyr-btn,
.globe-wrapper:fullscreen .layer-control .lyr-btn,
.globe-wrapper.fake-fs .layer-control .lyr-btn {
  color: rgba(255,255,255,0.7);
  border-color: rgba(255,255,255,0.18);
  background: rgba(255,255,255,0.06);
  padding: 8px 14px;
  font-size: 13px;
  flex-shrink: 0;
}

.globe-wrapper:-webkit-full-screen .layer-control .lyr-btn:hover,
.globe-wrapper:fullscreen .layer-control .lyr-btn:hover,
.globe-wrapper.fake-fs .layer-control .lyr-btn:hover {
  color: #fff;
  border-color: rgba(255,255,255,0.35);
  background: rgba(255,255,255,0.12);
}

.globe-wrapper:-webkit-full-screen .layer-control .lyr-btn.active,
.globe-wrapper:fullscreen .layer-control .lyr-btn.active,
.globe-wrapper.fake-fs .layer-control .lyr-btn.active {
  color: #93c5fd;
  border-color: rgba(147,197,253,0.4);
  background: rgba(59,130,246,0.18);
  box-shadow: 0 0 20px rgba(59,130,246,0.15);
}

.globe-wrapper:-webkit-full-screen .layer-control .lyr-dot,
.globe-wrapper:fullscreen .layer-control .lyr-dot,
.globe-wrapper.fake-fs .layer-control .lyr-dot {
  border-color: rgba(255,255,255,0.3);
}

/* Native fullscreen */
.globe-wrapper:-webkit-full-screen #map3d,
.globe-wrapper:fullscreen #map3d {
  height: 100vh !important;
  border-radius: 0 !important;
  margin: 0 !important;
}

/* Fake fullscreen for iOS */
.globe-wrapper.fake-fs {
  position: fixed !important;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 999998 !important;
  background: #000;
  overflow: hidden;
}

.globe-wrapper.fake-fs #map3d {
  height: 100vh !important;
  border-radius: 0 !important;
  margin: 0 !important;
  box-shadow: none;
}

.globe-wrapper.fake-fs .fs-btn {
  z-index: 999999;
}

/* Mobile fullscreen: compact */
@media (max-width: 600px) {
  .globe-wrapper:-webkit-full-screen .layer-control,
  .globe-wrapper:fullscreen .layer-control,
  .globe-wrapper.fake-fs .layer-control {
    bottom: 8px;
    padding: 10px 12px;
    border-radius: 10px;
    max-width: 95vw;
    max-height: 30vh;
  }

  .globe-wrapper:-webkit-full-screen .layer-control .layer-control-title,
  .globe-wrapper:fullscreen .layer-control .layer-control-title,
  .globe-wrapper.fake-fs .layer-control .layer-control-title {
    margin-bottom: 8px;
    font-size: 10px;
  }

  .globe-wrapper:-webkit-full-screen .layer-control .layer-grid,
  .globe-wrapper:fullscreen .layer-control .layer-grid,
  .globe-wrapper.fake-fs .layer-control .layer-grid {
    gap: 6px;
  }

  .globe-wrapper:-webkit-full-screen .layer-control .lyr-btn,
  .globe-wrapper:fullscreen .layer-control .lyr-btn,
  .globe-wrapper.fake-fs .layer-control .lyr-btn {
    padding: 6px 10px;
    font-size: 12px;
    gap: 6px;
    border-radius: 8px;
  }

  .globe-wrapper:-webkit-full-screen .layer-control .lyr-dot,
  .globe-wrapper:fullscreen .layer-control .lyr-dot,
  .globe-wrapper.fake-fs .layer-control .lyr-dot {
    width: 6px;
    height: 6px;
  }
}
</style>

<div class="globe-wrapper">
<link href="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Widgets/widgets.css" rel="stylesheet">
<script src="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Cesium.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>

<div style="position: relative;">
  <div id="map3d"></div>
  <button type="button" class="fs-btn" id="fullscreen-btn" title="full screen">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <polyline points="15 3 21 3 21 9"></polyline>
      <polyline points="9 21 3 21 3 15"></polyline>
      <line x1="21" y1="3" x2="14" y2="10"></line>
      <line x1="3" y1="21" x2="10" y2="14"></line>
    </svg>
  </button>
</div>

<div class="layer-control">
  <div class="layer-control-title">Layers</div>
  <div class="layer-grid">
    <button type="button" class="lyr-btn" data-layer="/data/NDVI.KMZ">
      <span class="lyr-dot"></span>
      <span>NDVI</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/lcc.kmz">
      <span class="lyr-dot"></span>
      <span>LCC</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/maize.kmz">
      <span class="lyr-dot"></span>
      <span>Corn</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/soybean.kmz">
      <span class="lyr-dot"></span>
      <span>Soybean</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/rice.kmz">
      <span class="lyr-dot"></span>
      <span>Rice</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/winter_wheat.kmz">
      <span class="lyr-dot"></span>
      <span>Winter Wheat</span>
    </button>
  </div>
</div>
</div>

<script>
function getMimeType(filename) {
  var ext = filename.split('.').pop().toLowerCase();
  var map = {
    'png': 'image/png', 'jpg': 'image/jpeg', 'jpeg': 'image/jpeg',
    'gif': 'image/gif', 'bmp': 'image/bmp', 'tif': 'image/tiff',
    'tiff': 'image/tiff', 'webp': 'image/webp', 'svg': 'image/svg+xml'
  };
  return map[ext] || 'application/octet-stream';
}

function loadKmzManually(viewer, kmzUrl) {
  return fetch(kmzUrl)
    .then(function(r) {
      if (!r.ok) throw new Error('HTTP ' + r.status);
      return r.arrayBuffer();
    })
    .then(function(buf) { return JSZip.loadAsync(buf); })
    .then(function(zip) {
      var kmlFile = null, kmlFileName = '', imageFiles = {};
      zip.forEach(function(path, file) {
        var lower = path.toLowerCase();
        if (lower.endsWith('.kml') && !kmlFile) {
          kmlFile = file;
          kmlFileName = path;
        } else if (/\.(png|jpg|jpeg|gif|bmp|tif|tiff|webp|svg)$/i.test(lower)) {
          imageFiles[path] = file;
        }
      });
      if (!kmlFile) throw new Error('KMZ 中未找到 KML');

      var imagePromises = [], imageUrlMap = {};
      Object.keys(imageFiles).forEach(function(imgPath) {
        imagePromises.push(
          imageFiles[imgPath].async('blob').then(function(blob) {
            imageUrlMap[imgPath] = URL.createObjectURL(new Blob([blob], {type: getMimeType(imgPath)}));
          })
        );
      });

      return Promise.all(imagePromises).then(function() {
        return kmlFile.async('string').then(function(kmlString) {
          Object.keys(imageUrlMap).forEach(function(imgPath) {
            var escaped = imgPath.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
            kmlString = kmlString.replace(new RegExp(escaped, 'g'), imageUrlMap[imgPath]);
            var baseName = imgPath.split('/').pop();
            if (baseName !== imgPath) {
              var escapedBase = baseName.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
              kmlString = kmlString.replace(new RegExp(escapedBase, 'g'), imageUrlMap[imgPath]);
            }
          });
          var kmlBlobUrl = URL.createObjectURL(new Blob([kmlString], {type: 'application/vnd.google-earth.kml+xml'}));
          return viewer.dataSources.add(
            Cesium.KmlDataSource.load(kmlBlobUrl, {
              camera: viewer.scene.camera,
              canvas: viewer.scene.canvas,
              clampToGround: true
            })
          ).then(function(ds) {
            ds._blobUrls = [kmlBlobUrl].concat(Object.values(imageUrlMap));
            return ds;
          });
        });
      });
    });
}

document.addEventListener("DOMContentLoaded", function() {
  try {
    Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJiNGU2MzgwZS1jNmM0LTQ4MDItOTc1ZS0wMTEyODNmOGNlMTYiLCJpZCI6NDAwMDcwLCJpYXQiOjE3NzI5Mzg2MDJ9.JTTgTyuiRGuJKpLArTT6KoAkzkC4TaB_M_FiOtWPwcU';
    var viewer = new Cesium.Viewer("map3d", {
      terrain: Cesium.Terrain.fromWorldTerrain(),
      baseLayerPicker: false,
      geocoder: false,
      animation: false,
      timeline: false,
      navigationHelpButton: false,
      fullscreenButton: false
    });

    var fsBtn = document.getElementById('fullscreen-btn');
    var wrapper = document.querySelector('.globe-wrapper');
    var isFakeFs = false;
    var savedScrollY = 0;

    function isNativeFullscreen() {
      return !!(document.fullscreenElement || document.webkitFullscreenElement);
    }

    function enterFakeFullscreen() {
      savedScrollY = window.scrollY;
      document.body.style.overflow = 'hidden';
      wrapper.classList.add('fake-fs');
      isFakeFs = true;
      viewer.resize();
    }

    function exitFakeFullscreen() {
      wrapper.classList.remove('fake-fs');
      document.body.style.overflow = '';
      isFakeFs = false;
      window.scrollTo(0, savedScrollY);
      viewer.resize();
    }

    if (fsBtn && wrapper) {
      fsBtn.addEventListener('click', function() {
        if (/iPad|iPhone|iPod/.test(navigator.userAgent) || (!wrapper.requestFullscreen && !wrapper.webkitRequestFullscreen)) {
          if (!isFakeFs) enterFakeFullscreen();
          else exitFakeFullscreen();
        } else {
          if (!isNativeFullscreen()) {
            if (wrapper.requestFullscreen) wrapper.requestFullscreen();
            else if (wrapper.webkitRequestFullscreen) wrapper.webkitRequestFullscreen();
          } else {
            if (document.exitFullscreen) document.exitFullscreen();
            else if (document.webkitExitFullscreen) document.webkitExitFullscreen();
          }
        }
      });

      document.addEventListener('fullscreenchange', function() {
        if (!document.fullscreenElement) viewer.resize();
      });
      document.addEventListener('webkitfullscreenchange', function() {
        if (!document.webkitFullscreenElement) viewer.resize();
      });
    }

    window.loaded3DLayers = {};
    var layerStates = {};

    function toggleLayer(btn) {
      var kmlUrl = btn.getAttribute('data-layer');
      var isActive = layerStates[kmlUrl] || false;

      if (!isActive) {
        layerStates[kmlUrl] = true;
        btn.classList.add('active');

        var isKmz = kmlUrl.toLowerCase().endsWith('.kmz');
        var loadPromise = isKmz
          ? loadKmzManually(viewer, kmlUrl)
          : viewer.dataSources.add(Cesium.KmlDataSource.load(kmlUrl, {
              camera: viewer.scene.camera, canvas: viewer.scene.canvas, clampToGround: true
            }));

        loadPromise.then(function(dataSource) {
          window.loaded3DLayers[kmlUrl] = dataSource;
        }).catch(function() {
          layerStates[kmlUrl] = false;
          btn.classList.remove('active');
        });
      } else {
        layerStates[kmlUrl] = false;
        btn.classList.remove('active');

        var activeLayer = window.loaded3DLayers[kmlUrl];
        if (activeLayer) {
          if (activeLayer._blobUrls) {
            activeLayer._blobUrls.forEach(function(url) { URL.revokeObjectURL(url); });
          }
          viewer.dataSources.remove(activeLayer);
          delete window.loaded3DLayers[kmlUrl];
        }
      }
    }

    var buttons = document.querySelectorAll('.lyr-btn');
    buttons.forEach(function(btn) {
      var touched = false;
      btn.addEventListener('touchend', function(e) {
        e.preventDefault();
        touched = true;
        toggleLayer(btn);
      }, {passive: false});
      btn.addEventListener('click', function(e) {
        if (touched) { touched = false; return; }
        toggleLayer(this);
      });
    });

  } catch (error) {
    document.getElementById("map3d").innerHTML =
      "<div style='padding: 20px; color: red;'>3D地球初始化失败：" + error.message + "</div>";
  }
});
</script>


## 3D Crop Globe
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,500;0,9..40,700;1,9..40,400&display=swap');
.crop-globe-wrapper * { box-sizing: border-box; font-family: 'DM Sans', sans-serif; }
.crop-globe-wrapper { position: relative; z-index: 1; }
.crop-globe-wrapper .crop-canvas-wrap { display: block; width: 100%; height: 600px; margin-top: 20px; border-radius: 12px; position: relative; z-index: 1; background-color: #0a0e17; overflow: hidden; box-shadow: 0 8px 32px rgba(0,0,0,0.3); cursor: grab; }
.crop-globe-wrapper .crop-canvas-wrap:active { cursor: grabbing; }
.crop-globe-wrapper .crop-canvas-wrap canvas { display: block; width: 100%; height: 100%; }
.crop-layer-control { position: relative; z-index: 2; margin-top: 16px; padding: 20px 24px; border-radius: 14px; border: 1px solid rgba(128,128,128,0.2); }
.crop-layer-control-title { display: flex; align-items: center; gap: 8px; margin-bottom: 16px; font-size: 13px; font-weight: 500; opacity: 0.45; text-transform: uppercase; letter-spacing: 1.5px; }
.crop-layer-control-title::before { content: ''; display: inline-block; width: 6px; height: 6px; border-radius: 50%; background: #22c55e; box-shadow: 0 0 8px rgba(34,197,94,0.4); }
.crop-layer-grid { display: flex; flex-wrap: wrap; gap: 10px; }
.crop-lyr-btn { display: flex; align-items: center; gap: 10px; padding: 10px 18px; border: 1px solid rgba(128,128,128,0.25); border-radius: 10px; cursor: pointer; background: transparent; font-size: 14px; font-weight: 500; font-family: 'DM Sans', sans-serif; color: inherit; opacity: 0.55; -webkit-appearance: none; appearance: none; touch-action: manipulation; transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1); }
.crop-lyr-btn:hover { opacity: 0.8; border-color: rgba(128,128,128,0.4); }
.crop-lyr-btn.active { opacity: 1; border-color: rgba(34,197,94,0.5); color: #22c55e; background: rgba(34,197,94,0.08); box-shadow: 0 0 16px rgba(34,197,94,0.08); }
.crop-lyr-btn.active .crop-lyr-dot { background: #22c55e; box-shadow: 0 0 8px rgba(34,197,94,0.5); border-color: transparent; }
.crop-lyr-dot { width: 8px; height: 8px; border-radius: 50%; border: 1.5px solid rgba(128,128,128,0.4); background: transparent; flex-shrink: 0; transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1); }
.crop-lyr-btn .crop-color-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; border: none; }
.crop-fs-btn { position: absolute; bottom: 16px; right: 16px; z-index: 2; width: 40px; height: 40px; border-radius: 10px; border: 1px solid rgba(255,255,255,0.1); background: rgba(0,0,0,0.5); backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px); cursor: pointer; display: flex; align-items: center; justify-content: center; color: rgba(255,255,255,0.6); transition: all 0.2s ease; touch-action: manipulation; -webkit-appearance: none; appearance: none; padding: 0; }
.crop-fs-btn:hover { background: rgba(0,0,0,0.7); color: #fff; border-color: rgba(255,255,255,0.2); }
.crop-fs-btn svg { width: 18px; height: 18px; }
.crop-globe-wrapper .crop-loading { position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%); color: rgba(255,255,255,0.4); font-size: 14px; letter-spacing: 1px; z-index: 3; pointer-events: none; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control, .crop-globe-wrapper:fullscreen .crop-layer-control, .crop-globe-wrapper.crop-fake-fs .crop-layer-control { position: fixed; bottom: 12px; left: 50%; transform: translateX(-50%); margin: 0; z-index: 999999; backdrop-filter: blur(24px) saturate(1.4); -webkit-backdrop-filter: blur(24px) saturate(1.4); background: rgba(255,255,255,0.18); border: 1px solid rgba(255,255,255,0.25); box-shadow: 0 8px 32px rgba(0,0,0,0.3), inset 0 1px 0 rgba(255,255,255,0.2); color: #fff; padding: 14px 18px; max-height: 35vh; display: flex; flex-direction: column; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-layer-control-title, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-layer-control-title, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-layer-control-title { color: rgba(255,255,255,0.5); opacity: 1; margin-bottom: 10px; font-size: 11px; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-lyr-btn { color: rgba(255,255,255,0.7); border-color: rgba(255,255,255,0.18); background: rgba(255,255,255,0.06); padding: 8px 14px; font-size: 13px; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-lyr-btn.active, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-lyr-btn.active, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-lyr-btn.active { color: #86efac; border-color: rgba(134,239,172,0.4); background: rgba(34,197,94,0.18); box-shadow: 0 0 20px rgba(34,197,94,0.15); }
.crop-globe-wrapper:-webkit-full-screen .crop-canvas-wrap, .crop-globe-wrapper:fullscreen .crop-canvas-wrap { height: 100vh !important; border-radius: 0 !important; margin: 0 !important; }
.crop-globe-wrapper.crop-fake-fs { position: fixed !important; top: 0; left: 0; width: 100vw; height: 100vh; z-index: 999998 !important; background: #000; overflow: hidden; }
.crop-globe-wrapper.crop-fake-fs .crop-canvas-wrap { height: 100vh !important; border-radius: 0 !important; margin: 0 !important; }
.crop-globe-wrapper.crop-fake-fs .crop-fs-btn { z-index: 999999; }
@media (max-width: 600px) { .crop-globe-wrapper:-webkit-full-screen .crop-layer-control, .crop-globe-wrapper:fullscreen .crop-layer-control, .crop-globe-wrapper.crop-fake-fs .crop-layer-control { bottom: 8px; padding: 10px 12px; max-width: 95vw; max-height: 30vh; } .crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-lyr-btn { padding: 6px 10px; font-size: 12px; gap: 6px; } }
</style>

<div class="crop-globe-wrapper">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>

<div class="crop-canvas-wrap" id="cropCanvasWrap">
  <div class="crop-loading" id="cropLoading" style="display:none;"></div>
  <button type="button" class="crop-fs-btn" id="crop-fullscreen-btn" title="full screen">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 3 21 3 21 9"></polyline><polyline points="9 21 3 21 3 15"></polyline><line x1="21" y1="3" x2="14" y2="10"></line><line x1="3" y1="21" x2="10" y2="14"></line></svg>
  </button>
</div>

<div class="crop-layer-control">
  <div class="crop-layer-control-title">3D Crop Layers</div>
  <div class="crop-layer-grid">
    <button type="button" class="crop-lyr-btn" data-crop-kmz="/data/maize.kmz" data-crop-type="corn"><span class="crop-color-dot" style="background:#FF9800;"></span><span>🌽 Corn</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-kmz="/data/rice.kmz" data-crop-type="rice"><span class="crop-color-dot" style="background:#8BC34A;"></span><span>🌾 Rice</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-kmz="/data/soybean.kmz" data-crop-type="soybean"><span class="crop-color-dot" style="background:#795548;"></span><span>🫘 Soybean</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-kmz="/data/winter_wheat.kmz" data-crop-type="wheat"><span class="crop-color-dot" style="background:#FFC107;"></span><span>🌾 Wheat</span></button>
  </div>
</div>
</div>

<script>
(function() {
  'use strict';

  /* ═══════════════════════════════════════════
     Three.js Scene
     ═══════════════════════════════════════════ */
  var container = document.getElementById('cropCanvasWrap');
  var W = container.clientWidth, H = container.clientHeight;
  var scene = new THREE.Scene();
  scene.background = new THREE.Color(0x020810);
  var camera = new THREE.PerspectiveCamera(45, W/H, 0.01, 100);
  camera.position.set(0, 0, 2.8);
  var renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(W, H);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.toneMapping = THREE.ACESFilmicToneMapping;
  renderer.toneMappingExposure = 1.1;
  container.appendChild(renderer.domElement);
  scene.add(new THREE.AmbientLight(0x556677, 0.5));
  var sun = new THREE.DirectionalLight(0xffeedd, 1.4); sun.position.set(5,3,5); scene.add(sun);
  var fill = new THREE.DirectionalLight(0x667788, 0.3); fill.position.set(-4,-1,-3); scene.add(fill);
  var globeGroup = new THREE.Group();
  scene.add(globeGroup);

  // Earth
  var earthMat = new THREE.MeshPhongMaterial({ color: 0xffffff, shininess: 15, specular: 0x111122 });
  globeGroup.add(new THREE.Mesh(new THREE.SphereGeometry(1, 72, 54), earthMat));
  // Auto-downsize texture for mobile
  var earthImg = new Image(); earthImg.crossOrigin = 'anonymous';
  earthImg.onload = function() {
    var maxSz = Math.min(renderer.capabilities.maxTextureSize, 4096);
    var w = earthImg.width, h = earthImg.height;
    if (w > maxSz || h > maxSz) {
      var r = Math.min(maxSz/w, maxSz/h), nw = Math.floor(w*r), nh = Math.floor(h*r);
      var cv = document.createElement('canvas'); cv.width=nw; cv.height=nh;
      cv.getContext('2d').drawImage(earthImg,0,0,nw,nh);
      var tex = new THREE.CanvasTexture(cv);
    } else { var tex = new THREE.Texture(earthImg); }
    tex.anisotropy = renderer.capabilities.getMaxAnisotropy();
    tex.needsUpdate = true; earthMat.map = tex; earthMat.needsUpdate = true;
  };
  earthImg.src = '/images/earth.jpg';
  // Atmosphere
  globeGroup.add(new THREE.Mesh(new THREE.SphereGeometry(1.015,48,36), new THREE.MeshPhongMaterial({color:0x6699cc,transparent:true,opacity:0.06})));
  globeGroup.add(new THREE.Mesh(new THREE.SphereGeometry(1.06,32,24), new THREE.MeshBasicMaterial({color:0x3366aa,transparent:true,opacity:0.045,side:THREE.BackSide})));
  // Stars
  var sp=[]; for(var i=0;i<2000;i++){var r=20+Math.random()*50,th=Math.random()*Math.PI*2,ph=Math.acos(2*Math.random()-1);sp.push(r*Math.sin(ph)*Math.cos(th),r*Math.sin(ph)*Math.sin(th),r*Math.cos(ph));}
  var sg=new THREE.BufferGeometry(); sg.setAttribute('position',new THREE.Float32BufferAttribute(sp,3));
  scene.add(new THREE.Points(sg, new THREE.PointsMaterial({color:0xffffff,size:0.08,transparent:true,opacity:0.7})));

  function ll2v(lat,lon,r){var p=(90-lat)*Math.PI/180,t=(lon+180)*Math.PI/180;return new THREE.Vector3(-r*Math.sin(p)*Math.cos(t),r*Math.cos(p),r*Math.sin(p)*Math.sin(t));}

  /* ═══════════════════════════════════════════
     Geometry helpers
     ═══════════════════════════════════════════ */
  function mergeGeos(geos) {
    var totalV=0; geos.forEach(function(g){g.computeVertexNormals();totalV+=g.attributes.position.count;});
    var pos=new Float32Array(totalV*3),nrm=new Float32Array(totalV*3),idx=[],vOff=0;
    geos.forEach(function(g){
      pos.set(g.attributes.position.array,vOff*3); nrm.set(g.attributes.normal.array,vOff*3);
      if(g.index){for(var i=0;i<g.index.count;i++)idx.push(g.index.array[i]+vOff);}
      else{for(var i=0;i<g.attributes.position.count;i++)idx.push(vOff+i);}
      vOff+=g.attributes.position.count;
    });
    var mg=new THREE.BufferGeometry();
    mg.setAttribute('position',new THREE.BufferAttribute(pos,3));
    mg.setAttribute('normal',new THREE.BufferAttribute(nrm,3));
    mg.setIndex(idx); return mg;
  }

  function bakeColors(parts, brownCount, greenHex, brownHex) {
    var brown=new THREE.Color(brownHex||0x6D4C41), green=new THREE.Color(greenHex||0x2e7d32);
    var totalV=0; parts.forEach(function(g){totalV+=g.attributes.position.count;});
    var colors=new Float32Array(totalV*3), off=0;
    for(var gi=0;gi<parts.length;gi++){
      var vc=parts[gi].attributes.position.count, col=gi<brownCount?brown:green;
      for(var v=0;v<vc;v++){var rv=0.9+Math.sin(v*7.3+gi)*0.1;
        colors[(off+v)*3]=col.r*rv;colors[(off+v)*3+1]=col.g*rv;colors[(off+v)*3+2]=col.b*rv;}
      off+=vc;
    } return colors;
  }

  /* ═══════════════════════════════════════════
     Crop model builders
     ═══════════════════════════════════════════ */
  // CORN: thick stalk, wide arching leaves, 1-2 cobs, tassel
  function cornParts() {
    var p=[];
    p.push((function(){var g=new THREE.CylinderGeometry(0.05,0.08,2.6,8);g.translate(0,1.3,0);return g;})()); // stalk
    // brown=1
    for(var i=0;i<7;i++){var lf=new THREE.BoxGeometry(1.0,0.025,0.12,5,1,1);var a=lf.attributes.position.array;
      for(var j=0;j<a.length;j+=3){a[j+1]+=a[j]*a[j]*0.25;a[j+2]+=a[j]*0.06;}
      lf.rotateY(i*Math.PI/3.5+(i%2)*0.3);lf.translate(0,0.5+i*0.3,0);p.push(lf);}
    // cobs (yellow-ish - still green parts array, but we handle in bake)
    var c1=new THREE.SphereGeometry(0.1,8,6);c1.scale(1,2.2,1);c1.translate(0.12,1.6,0);p.push(c1);
    var c2=new THREE.SphereGeometry(0.08,8,6);c2.scale(1,1.8,1);c2.translate(-0.1,1.95,0.05);p.push(c2);
    // husk
    var h1=new THREE.SphereGeometry(0.07,6,4);h1.scale(0.5,1.5,0.3);h1.translate(0.18,1.6,0.04);p.push(h1);
    // tassel
    for(var t=0;t<5;t++){var ts=new THREE.CylinderGeometry(0.006,0.002,0.3,3);ts.rotateZ((t-2)*0.2);ts.rotateY(t*1.2);ts.translate(0,2.7,0);p.push(ts);}
    return p;
  }

  // RICE: thin tillers from base, narrow drooping leaves, drooping panicles
  function riceParts() {
    var p=[];
    // brown=0 (rice stalks are green)
    var tl=[[-0.06,0],[0.05,0.04],[0,-0.05],[0.08,-0.02],[-0.03,0.06],[-0.07,-0.04],[0.04,-0.06]];
    for(var s=0;s<tl.length;s++){
      var h=1.1+(s%3)*0.15;
      var stk=new THREE.CylinderGeometry(0.01,0.018,h,4);stk.translate(tl[s][0],h/2,tl[s][1]);p.push(stk);
      // narrow leaf
      var lf=new THREE.BoxGeometry(0.45,0.012,0.03,4,1,1);var a=lf.attributes.position.array;
      for(var j=0;j<a.length;j+=3)a[j+1]+=a[j]*a[j]*0.3;
      lf.rotateY(s*0.9+0.5);lf.translate(tl[s][0],0.25+s*0.1,tl[s][1]);p.push(lf);
      // drooping panicle
      var pn=new THREE.SphereGeometry(0.02,5,4);pn.scale(0.8,3,0.8);pn.rotateZ(0.5+s*0.1);pn.rotateY(s*0.9);
      pn.translate(tl[s][0]+0.06,h+0.04,tl[s][1]);p.push(pn);
    }
    return p;
  }

  // SOYBEAN: short branching, trifoliate leaves, pods
  function soybeanParts() {
    var p=[];
    p.push((function(){var g=new THREE.CylinderGeometry(0.025,0.04,0.9,6);g.translate(0,0.45,0);return g;})()); // stem
    // brown=1
    // branches
    var br=[[0.3,0.5,0.1],[-0.25,0.4,-0.08],[0.2,0.65,-0.1],[-0.2,0.6,0.12]];
    for(var b=0;b<4;b++){var bg=new THREE.CylinderGeometry(0.01,0.018,0.25,4);
      bg.rotateZ(b%2===0?0.8:-0.8);bg.translate((b%2===0?1:-1)*0.12,0.35+b*0.12,(b<2?0.05:-0.05));p.push(bg);}
    // trifoliate leaves
    var lpos=[[0.1,0.7,0.04],[-0.08,0.6,-0.03],[0.06,0.85,-0.05],[-0.05,0.8,0.07],[0.12,0.5,-0.02],[-0.1,0.45,0.05],[0,0.92,0]];
    for(var l=0;l<lpos.length;l++){for(var t=0;t<3;t++){
      var lf=new THREE.SphereGeometry(0.05,5,4);lf.scale(1.3,0.12,0.8);
      var ang=(t/3)*Math.PI*2+l*0.7;
      lf.translate(lpos[l][0]+Math.cos(ang)*0.03,lpos[l][1],lpos[l][2]+Math.sin(ang)*0.03);p.push(lf);}}
    // pods
    var pp=[[0.05,0.38,0.02],[-0.03,0.48,-0.02],[0.07,0.58,0.01],[-0.05,0.68,0.03],[0.02,0.78,-0.02],[-0.04,0.32,0.04]];
    for(var pd=0;pd<pp.length;pd++){var pg=new THREE.SphereGeometry(0.015,4,3);pg.scale(0.5,2.5,0.5);
      pg.rotateZ(0.3*(pd%2===0?1:-1));pg.translate(pp[pd][0],pp[pd][1],pp[pd][2]);p.push(pg);}
    return p;
  }

  // WHEAT: upright stalks, plump heads with awns
  function wheatParts() {
    var p=[];
    // brown=0 (wheat is all golden-green)
    var st=[[-0.05,0],[0.04,0.04],[0,-0.04],[0.07,-0.02],[-0.03,0.05],[-0.06,-0.03]];
    for(var s=0;s<st.length;s++){
      var h=1.5+(s%3)*0.1;
      var stk=new THREE.CylinderGeometry(0.015,0.025,h,5);stk.translate(st[s][0],h/2,st[s][1]);p.push(stk);
      // wheat head - elongated ellipsoid
      var hd=new THREE.SphereGeometry(0.045,6,5);hd.scale(0.7,2.2,0.7);hd.translate(st[s][0],h+0.08,st[s][1]);p.push(hd);
      // awns (3 per head)
      for(var a=0;a<3;a++){var aw=new THREE.CylinderGeometry(0.004,0.001,0.12,3);
        aw.rotateZ(0.3*(a-1));aw.translate(st[s][0],h+0.15+a*0.03,st[s][1]);p.push(aw);}
    }
    return p;
  }

  // Build colored geos
  var geoConfigs=[
    {fn:cornParts,    brown:1, green:0x4a7c23, brownHex:0x6D4C41},
    {fn:riceParts,    brown:0, green:0x6aaa30, brownHex:0x5D4037},
    {fn:soybeanParts, brown:1, green:0x558b2f, brownHex:0x6D4C41},
    {fn:wheatParts,   brown:0, green:0xb8a520, brownHex:0x8d6e27}, // golden-green for wheat
  ];
  var coloredGeos=geoConfigs.map(function(cfg){
    var parts=cfg.fn(),merged=mergeGeos(parts);
    merged.setAttribute('color',new THREE.BufferAttribute(bakeColors(parts,cfg.brown,cfg.green,cfg.brownHex),3));
    return merged;
  });

  var CROP_TYPES={
    corn:    {geo:coloredGeos[0], sc:0.010, label:'Corn'},
    rice:    {geo:coloredGeos[1], sc:0.008, label:'Rice'},
    soybean: {geo:coloredGeos[2], sc:0.007, label:'Soybean'},
    wheat:   {geo:coloredGeos[3], sc:0.009, label:'Wheat'},
  };

  /* ═══════════════════════════════════════════
     KMZ → sample with color ramp density
     Color ramp: Yellow(low) → Green(mid) → Blue/Purple(high)
     percentage = (255-R + B) / 510 * 100
     ═══════════════════════════════════════════ */
  function extractImageFromKMZ(url){
    return fetch(url).then(function(r){if(!r.ok)throw new Error('HTTP '+r.status);return r.arrayBuffer();})
    .then(function(buf){return JSZip.loadAsync(buf);})
    .then(function(zip){
      var best=null;
      zip.forEach(function(path,file){if(/\.(png|jpg|jpeg)$/i.test(path)){if(!best||file._data.uncompressedSize>(best._data?best._data.uncompressedSize:0))best=file;}});
      if(!best){zip.forEach(function(path,file){if(/\.(png|jpg|jpeg)$/i.test(path))best=file;});}
      if(!best)throw new Error('No image in KMZ');
      return best.async('blob');
    }).then(function(blob){return new Promise(function(res,rej){var img=new Image();img.onload=function(){res(img);};img.onerror=rej;img.src=URL.createObjectURL(blob);});});
  }

  function sampleKMZImage(img, stepDeg) {
    stepDeg = stepDeg || 1.0;
    var maxDim = Math.min(img.width, 2048);
    var ratio = maxDim / img.width;
    var w = Math.floor(img.width * ratio), h = Math.floor(img.height * ratio);
    var cv = document.createElement('canvas'); cv.width = w; cv.height = h;
    var ctx = cv.getContext('2d'); ctx.drawImage(img, 0, 0, w, h);
    var data = ctx.getImageData(0, 0, w, h).data;
    var points = [];

    for (var lat = 85; lat > -70; lat -= stepDeg) {
      for (var lon = -180; lon < 180; lon += stepDeg) {
        var px = Math.floor(((lon+180)/360)*w) % w;
        var py = Math.max(0, Math.min(h-1, Math.floor(((90-lat)/180)*h)));
        var idx = (py*w+px)*4;
        var r=data[idx], g=data[idx+1], b=data[idx+2], a=data[idx+3];

        if (a < 128) continue; // transparent = no crop

        // Color ramp: Yellow(R≈255,G≈255)=low, Green(G>200,R<80)=mid, Blue(B>100,R<80)=high
        var pct = Math.min(100, Math.max(0, ((255-r)+b) / 510 * 100));
        if (pct < 5) continue; // skip near-zero

        // Density: high% → more sub-points, low% → fewer
        var numPts = pct > 60 ? 3 : (pct > 30 ? 2 : 1);

        for (var d = 0; d < numPts; d++) {
          var jLat = lat + (Math.sin(d*13.7+lon)*0.5-0.25) * stepDeg * 0.8;
          var jLon = lon + (Math.cos(d*7.3+lat)*0.5-0.25) * stepDeg * 0.8;
          points.push([
            Math.round(jLat*100)/100,
            Math.round(jLon*100)/100,
            Math.round(pct) // percentage for size scaling
          ]);
        }
      }
    }
    return points;
  }

  /* ═══════════════════════════════════════════
     Render crop layer
     ═══════════════════════════════════════════ */
  var meshGroups = {}, cropCache = {};

  function renderCrop(cropType, points) {
    removeCrop(cropType);
    var cfg = CROP_TYPES[cropType];
    if (!cfg || !points.length) return;
    meshGroups[cropType] = [];
    var dummy = new THREE.Object3D();
    var n = points.length;
    var mat = new THREE.MeshPhongMaterial({ vertexColors:true, shininess:20, flatShading:false });
    var mesh = new THREE.InstancedMesh(cfg.geo, mat, n);

    for (var i = 0; i < n; i++) {
      var lat = points[i][0], lon = points[i][1], pct = points[i][2];
      // Scale based on percentage: high% = bigger, low% = smaller
      var pctNorm = pct / 100; // 0..1
      var s = cfg.sc * (0.7 + pctNorm * 0.8) * (0.85 + ((i*7)%100)/100*0.3);

      var pos = ll2v(lat, lon, 1.003);
      var norm = pos.clone().normalize();

      dummy.position.copy(pos);
      dummy.position.addScaledVector(norm, s * 0.1);
      dummy.up.copy(norm);
      var tangent = new THREE.Vector3(-norm.z, 0, norm.x).normalize();
      if (tangent.lengthSq() < 0.001) tangent.set(1, 0, 0);
      dummy.lookAt(pos.clone().add(tangent));
      dummy.rotateOnAxis(new THREE.Vector3(0,1,0), ((i*37)%628)/100);
      dummy.scale.set(s, s*(0.8+pctNorm*0.4), s);
      dummy.updateMatrix();
      mesh.setMatrixAt(i, dummy.matrix);
    }
    mesh.instanceMatrix.needsUpdate = true;
    globeGroup.add(mesh);
    meshGroups[cropType] = [mesh];
  }

  function removeCrop(type) {
    if(meshGroups[type]){meshGroups[type].forEach(function(m){globeGroup.remove(m);});delete meshGroups[type];}
  }

  /* ═══════════════════════════════════════════
     Button handlers
     ═══════════════════════════════════════════ */
  var btns = document.querySelectorAll('.crop-lyr-btn');
  btns.forEach(function(btn) {
    var touched = false;
    btn.addEventListener('touchend', function(e){e.preventDefault();touched=true;toggle(btn);},{passive:false});
    btn.addEventListener('click', function(){if(touched){touched=false;return;}toggle(btn);});
  });

  function toggle(btn) {
    var type = btn.getAttribute('data-crop-type');
    var url = btn.getAttribute('data-crop-kmz');
    if (btn.classList.contains('active')) {
      btn.classList.remove('active'); removeCrop(type); return;
    }
    btn.classList.add('active');
    if (cropCache[type]) { renderCrop(type, cropCache[type]); return; }
    // Load from KMZ
    var ld = document.getElementById('cropLoading');
    ld.style.display=''; ld.textContent='Loading '+CROP_TYPES[type].label+'…';
    extractImageFromKMZ(url).then(function(img){
      var pts = sampleKMZImage(img, 1.0);
      cropCache[type] = pts;
      console.log('[3D Crops] '+type+': '+pts.length+' points');
      renderCrop(type, pts);
      ld.style.display='none';
    }).catch(function(e){
      console.error(e); btn.classList.remove('active');
      ld.textContent='Failed: '+e.message;
      setTimeout(function(){ld.style.display='none';},3000);
    });
  }

  /* ═══════════════════════════════════════════
     Interaction
     ═══════════════════════════════════════════ */
  var isDrag=false,prev={x:0,y:0},vel={x:0,y:0};
  var el=renderer.domElement;
  el.addEventListener('pointerdown',function(e){isDrag=true;prev={x:e.clientX,y:e.clientY};});
  el.addEventListener('pointermove',function(e){if(!isDrag)return;vel.y=(e.clientX-prev.x)*0.004;vel.x=(e.clientY-prev.y)*0.004;prev={x:e.clientX,y:e.clientY};});
  el.addEventListener('pointerup',function(){isDrag=false;});
  el.addEventListener('pointerleave',function(){isDrag=false;});
  el.addEventListener('wheel',function(e){camera.position.z=Math.max(1.3,Math.min(5,camera.position.z+e.deltaY*0.002));},{passive:true});
  var ld2=0;
  el.addEventListener('touchstart',function(e){if(e.touches.length===2)ld2=Math.hypot(e.touches[0].clientX-e.touches[1].clientX,e.touches[0].clientY-e.touches[1].clientY);},{passive:true});
  el.addEventListener('touchmove',function(e){if(e.touches.length===2){var d=Math.hypot(e.touches[0].clientX-e.touches[1].clientX,e.touches[0].clientY-e.touches[1].clientY);if(ld2>0)camera.position.z=Math.max(1.3,Math.min(5,camera.position.z-(d-ld2)*0.005));ld2=d;}},{passive:true});
  el.addEventListener('touchend',function(){ld2=0;},{passive:true});

  function animate(){
    requestAnimationFrame(animate);
    if(!isDrag){vel.y*=0.95;vel.x*=0.95;}
    globeGroup.rotation.y+=vel.y; globeGroup.rotation.x+=vel.x;
    globeGroup.rotation.x=Math.max(-1.2,Math.min(1.2,globeGroup.rotation.x));
    vel.x*=0.96;vel.y*=0.98;
    renderer.render(scene,camera);
  }
  animate();

  function onResize(){var w=container.clientWidth,h=container.clientHeight;camera.aspect=w/h;camera.updateProjectionMatrix();renderer.setSize(w,h);}
  window.addEventListener('resize',onResize);

  // Fullscreen
  var fsBtn=document.getElementById('crop-fullscreen-btn'),wrap=document.querySelector('.crop-globe-wrapper'),fakeFs=false,saveY=0;
  if(fsBtn&&wrap){fsBtn.addEventListener('click',function(){
    if(/iPad|iPhone|iPod/.test(navigator.userAgent)||(!wrap.requestFullscreen&&!wrap.webkitRequestFullscreen)){if(!fakeFs){saveY=window.scrollY;document.body.style.overflow='hidden';wrap.classList.add('crop-fake-fs');fakeFs=true;}else{wrap.classList.remove('crop-fake-fs');document.body.style.overflow='';fakeFs=false;window.scrollTo(0,saveY);}}
    else{if(!(document.fullscreenElement||document.webkitFullscreenElement))(wrap.requestFullscreen||wrap.webkitRequestFullscreen).call(wrap);else(document.exitFullscreen||document.webkitExitFullscreen).call(document);}
    setTimeout(onResize,100);
  });document.addEventListener('fullscreenchange',function(){setTimeout(onResize,100);});document.addEventListener('webkitfullscreenchange',function(){setTimeout(onResize,100);});}
})();
</script>