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
.crop-globe-wrapper #cropMap3d { display: block; width: 100%; height: 600px; margin-top: 20px; border-radius: 12px; position: relative; z-index: 1; background-color: #0a0e17; overflow: hidden; box-shadow: 0 8px 32px rgba(0,0,0,0.3); }
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
.crop-stats { display: flex; gap: 16px; margin-top: 12px; padding-top: 12px; border-top: 1px solid rgba(128,128,128,0.1); font-size: 12px; opacity: 0.35; transition: opacity 0.3s; flex-wrap: wrap; }
.crop-stats.visible { opacity: 0.6; }
.crop-stats span { display: flex; align-items: center; gap: 6px; }
.crop-fs-btn { position: absolute; bottom: 36px; right: 16px; z-index: 2; width: 40px; height: 40px; border-radius: 10px; border: 1px solid rgba(255,255,255,0.1); background: rgba(0,0,0,0.5); backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px); cursor: pointer; display: flex; align-items: center; justify-content: center; color: rgba(255,255,255,0.6); transition: all 0.2s ease; touch-action: manipulation; -webkit-appearance: none; appearance: none; padding: 0; }
.crop-fs-btn:hover { background: rgba(0,0,0,0.7); color: #fff; border-color: rgba(255,255,255,0.2); }
.crop-fs-btn svg { width: 18px; height: 18px; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control, .crop-globe-wrapper:fullscreen .crop-layer-control, .crop-globe-wrapper.crop-fake-fs .crop-layer-control { position: fixed; bottom: 12px; left: 50%; transform: translateX(-50%); margin: 0; z-index: 999999; backdrop-filter: blur(24px) saturate(1.4); -webkit-backdrop-filter: blur(24px) saturate(1.4); background: rgba(255,255,255,0.18); border: 1px solid rgba(255,255,255,0.25); box-shadow: 0 8px 32px rgba(0,0,0,0.3), inset 0 1px 0 rgba(255,255,255,0.2); color: #fff; padding: 14px 18px; max-height: 35vh; display: flex; flex-direction: column; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-layer-control-title, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-layer-control-title, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-layer-control-title { color: rgba(255,255,255,0.5); opacity: 1; margin-bottom: 10px; font-size: 11px; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-lyr-btn { color: rgba(255,255,255,0.7); border-color: rgba(255,255,255,0.18); background: rgba(255,255,255,0.06); padding: 8px 14px; font-size: 13px; }
.crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-lyr-btn.active, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-lyr-btn.active, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-lyr-btn.active { color: #86efac; border-color: rgba(134,239,172,0.4); background: rgba(34,197,94,0.18); box-shadow: 0 0 20px rgba(34,197,94,0.15); }
.crop-globe-wrapper:-webkit-full-screen #cropMap3d, .crop-globe-wrapper:fullscreen #cropMap3d { height: 100vh !important; border-radius: 0 !important; margin: 0 !important; }
.crop-globe-wrapper.crop-fake-fs { position: fixed !important; top: 0; left: 0; width: 100vw; height: 100vh; z-index: 999998 !important; background: #000; overflow: hidden; }
.crop-globe-wrapper.crop-fake-fs #cropMap3d { height: 100vh !important; border-radius: 0 !important; margin: 0 !important; }
.crop-globe-wrapper.crop-fake-fs .crop-fs-btn { z-index: 999999; }
@media (max-width: 600px) { .crop-globe-wrapper:-webkit-full-screen .crop-layer-control, .crop-globe-wrapper:fullscreen .crop-layer-control, .crop-globe-wrapper.crop-fake-fs .crop-layer-control { bottom: 8px; padding: 10px 12px; border-radius: 10px; max-width: 95vw; max-height: 30vh; } .crop-globe-wrapper:-webkit-full-screen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper:fullscreen .crop-layer-control .crop-lyr-btn, .crop-globe-wrapper.crop-fake-fs .crop-layer-control .crop-lyr-btn { padding: 6px 10px; font-size: 12px; gap: 6px; } }
</style>

<div class="crop-globe-wrapper">
<link href="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Widgets/widgets.css" rel="stylesheet">
<script src="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Cesium.js"></script>

<div style="position: relative;">
  <div id="cropMap3d"></div>
  <button type="button" class="crop-fs-btn" id="crop-fullscreen-btn" title="full screen">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 3 21 3 21 9"></polyline><polyline points="9 21 3 21 3 15"></polyline><line x1="21" y1="3" x2="14" y2="10"></line><line x1="3" y1="21" x2="10" y2="14"></line></svg>
  </button>
</div>

<div class="crop-layer-control">
  <div class="crop-layer-control-title">3D Crop Layers</div>
  <div class="crop-layer-grid">
    <button type="button" class="crop-lyr-btn active" data-crop-type="all"><span class="crop-lyr-dot"></span><span>All</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-type="tropical"><span class="crop-color-dot" style="background:#228B22;"></span><span>Tropical</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-type="forest"><span class="crop-color-dot" style="background:#43a047;"></span><span>Forest</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-type="crop"><span class="crop-color-dot" style="background:#8bc34a;"></span><span>Cropland</span></button>
    <button type="button" class="crop-lyr-btn" data-crop-type="grass"><span class="crop-color-dot" style="background:#a5d6a7;"></span><span>Grassland</span></button>
  </div>
  <div class="crop-stats" id="cropStats">
    <span>🌱 <strong id="cropCount">—</strong> rendered</span>
    <span>📏 LOD: <strong id="cropLod">—</strong></span>
    <span>📊 MODIS NDVI</span>
  </div>
</div>
</div>

<script>
(function() {
  'use strict';

  var TYPES = ['tropical','forest','crop','grass'];
  var STYLES = {
    tropical: { colors: ['#1a5c1a','#1e6e1e','#228B22','#0d5e0d','#2d8a2d'], trunk: '#5D4037', baseR: 500, baseH: 1500, shape: 'tree' },
    forest:   { colors: ['#2e7d32','#388e3c','#43a047','#1b5e20','#4caf50'], trunk: '#6D4C41', baseR: 400, baseH: 1100, shape: 'tree' },
    crop:     { colors: ['#8bc34a','#9ccc65','#aed581','#7cb342','#c0ca33'], trunk: '#8d6e27', baseR: 250, baseH: 600,  shape: 'wheat' },
    grass:    { colors: ['#a5d6a7','#81c784','#c5e1a5','#aed581','#dce775'], trunk: null,      baseR: 300, baseH: 250,  shape: 'bush' }
  };

  // LOD: limit total geometry count at each zoom level
  var LODS = [
    { max: 100000,   rate: 1,  mul: 1.0,  lbl: 'Ultra',  cap: 2000 },
    { max: 500000,   rate: 3,  mul: 2.0,  lbl: 'High',   cap: 1500 },
    { max: 2000000,  rate: 8,  mul: 4.0,  lbl: 'Medium', cap: 800  },
    { max: 8000000,  rate: 20, mul: 8.0,  lbl: 'Low',    cap: 400  },
    { max: Infinity, rate: 50, mul: 14.0, lbl: 'Min',    cap: 200  }
  ];

  function getLod(h) { for (var i = 0; i < LODS.length; i++) { if (h < LODS[i].max) return LODS[i]; } return LODS[LODS.length-1]; }

  var viewer = null, allPts = null, activeFilter = 'all';
  var curPrims = [], curLod = '', lodTimer = null;

  function clearPrims() {
    for (var i = 0; i < curPrims.length; i++) viewer.scene.primitives.remove(curPrims[i]);
    curPrims = [];
  }

  function render(pts, filter, lod) {
    clearPrims();
    var ch = viewer.camera.positionCartographic.height;
    lod = lod || getLod(ch);
    var count = 0, geoms = [], trunks = [];

    for (var i = 0; i < pts.length && count < lod.cap; i++) {
      if (i % lod.rate !== 0) continue;
      var p = pts[i], ptype = TYPES[p[2]] || 'grass';
      if (filter !== 'all' && ptype !== filter) continue;

      var st = STYLES[ptype], lat = p[0], lon = p[1], sc = p[4]/100;
      var col = Cesium.Color.fromCssColorString(st.colors[i % st.colors.length]);
      var rr = 0.7 + ((i*7)%100)/100*0.6, rh = 0.7 + ((i*13)%100)/100*0.6;
      var R = st.baseR*(0.5+sc*0.8)*lod.mul*rr;
      var H = st.baseH*(0.4+sc*0.8)*lod.mul*rh;

      var mm = function(ln, lt, alt) {
        return Cesium.Transforms.headingPitchRollToFixedFrame(
          Cesium.Cartesian3.fromDegrees(ln, lt, alt),
          new Cesium.HeadingPitchRoll(0,0,0)
        );
      };

      if (st.shape === 'tree') {
        var tH = H*0.3, cH = H*0.7;
        trunks.push(new Cesium.GeometryInstance({
          geometry: new Cesium.CylinderGeometry({ length: tH, topRadius: R*0.06, bottomRadius: R*0.1, slices: 5 }),
          modelMatrix: mm(lon, lat, tH/2),
          attributes: { color: Cesium.ColorGeometryInstanceAttribute.fromColor(Cesium.Color.fromCssColorString(st.trunk)) }
        }));
        geoms.push(new Cesium.GeometryInstance({
          geometry: new Cesium.CylinderGeometry({ length: cH, topRadius: 0, bottomRadius: R, slices: 6 }),
          modelMatrix: mm(lon, lat, tH + cH/2),
          attributes: { color: Cesium.ColorGeometryInstanceAttribute.fromColor(col.withAlpha(0.9)) }
        }));
        count++;
      } else if (st.shape === 'wheat') {
        var sH = H*0.65;
        trunks.push(new Cesium.GeometryInstance({
          geometry: new Cesium.CylinderGeometry({ length: sH, topRadius: R*0.025, bottomRadius: R*0.05, slices: 4 }),
          modelMatrix: mm(lon, lat, sH/2),
          attributes: { color: Cesium.ColorGeometryInstanceAttribute.fromColor(Cesium.Color.fromCssColorString(st.trunk)) }
        }));
        geoms.push(new Cesium.GeometryInstance({
          geometry: new Cesium.EllipsoidGeometry({ radii: new Cesium.Cartesian3(R*0.3, R*0.3, H*0.2), stackPartitions: 5, slicePartitions: 5 }),
          modelMatrix: mm(lon, lat, sH + H*0.18),
          attributes: { color: Cesium.ColorGeometryInstanceAttribute.fromColor(col.withAlpha(0.88)) }
        }));
        count++;
      } else {
        geoms.push(new Cesium.GeometryInstance({
          geometry: new Cesium.EllipsoidGeometry({ radii: new Cesium.Cartesian3(R, R*0.8, H*0.5), stackPartitions: 5, slicePartitions: 5 }),
          modelMatrix: mm(lon, lat, H*0.35),
          attributes: { color: Cesium.ColorGeometryInstanceAttribute.fromColor(col.withAlpha(0.85)) }
        }));
        count++;
      }
    }

    if (geoms.length > 0) {
      var gp = new Cesium.Primitive({ geometryInstances: geoms, appearance: new Cesium.PerInstanceColorAppearance({ closed: true, translucent: true }), asynchronous: true });
      viewer.scene.primitives.add(gp); curPrims.push(gp);
    }
    if (trunks.length > 0) {
      var tp = new Cesium.Primitive({ geometryInstances: trunks, appearance: new Cesium.PerInstanceColorAppearance({ closed: true, translucent: false }), asynchronous: true });
      viewer.scene.primitives.add(tp); curPrims.push(tp);
    }

    document.getElementById('cropCount').textContent = count.toLocaleString();
    document.getElementById('cropLod').textContent = lod.lbl + ' (' + Math.round(ch/1000) + 'km)';
    document.getElementById('cropStats').classList.add('visible');
    curLod = lod.lbl;
    console.log('[3D Crops] ' + count + ' objects @ ' + lod.lbl);
  }

  function startLod() {
    viewer.camera.changed.addEventListener(function() {
      if (!allPts) return;
      var ch = viewer.camera.positionCartographic.height, nl = getLod(ch);
      if (nl.lbl !== curLod) { if (lodTimer) clearTimeout(lodTimer); lodTimer = setTimeout(function() { render(allPts, activeFilter, nl); }, 500); }
      document.getElementById('cropLod').textContent = nl.lbl + ' (' + Math.round(ch/1000) + 'km)';
    });
    viewer.camera.percentageChanged = 0.15;
  }

  document.addEventListener("DOMContentLoaded", function() {
    try {
      Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJiNGU2MzgwZS1jNmM0LTQ4MDItOTc1ZS0wMTEyODNmOGNlMTYiLCJpZCI6NDAwMDcwLCJpYXQiOjE3NzI5Mzg2MDJ9.JTTgTyuiRGuJKpLArTT6KoAkzkC4TaB_M_FiOtWPwcU';
      viewer = new Cesium.Viewer("cropMap3d", { terrain: Cesium.Terrain.fromWorldTerrain(), baseLayerPicker: false, geocoder: false, animation: false, timeline: false, navigationHelpButton: false, fullscreenButton: false });

      var fsBtn = document.getElementById('crop-fullscreen-btn'), wrap = document.querySelector('.crop-globe-wrapper'), fakeFs = false, saveY = 0;
      if (fsBtn && wrap) {
        fsBtn.addEventListener('click', function() {
          if (/iPad|iPhone|iPod/.test(navigator.userAgent) || (!wrap.requestFullscreen && !wrap.webkitRequestFullscreen)) {
            if (!fakeFs) { saveY = window.scrollY; document.body.style.overflow = 'hidden'; wrap.classList.add('crop-fake-fs'); fakeFs = true; }
            else { wrap.classList.remove('crop-fake-fs'); document.body.style.overflow = ''; fakeFs = false; window.scrollTo(0, saveY); }
            viewer.resize();
          } else {
            if (!(document.fullscreenElement || document.webkitFullscreenElement)) (wrap.requestFullscreen || wrap.webkitRequestFullscreen).call(wrap);
            else (document.exitFullscreen || document.webkitExitFullscreen).call(document);
          }
        });
        document.addEventListener('fullscreenchange', function() { if (!document.fullscreenElement) viewer.resize(); });
        document.addEventListener('webkitfullscreenchange', function() { if (!document.webkitFullscreenElement) viewer.resize(); });
      }

      // ★ 改成你的 crops.json 路径
      fetch('/data/crops.json')
        .then(function(r) { if (!r.ok) throw new Error('HTTP ' + r.status); return r.json(); })
        .then(function(data) { allPts = data; console.log('[3D Crops] Loaded ' + data.length); render(data, 'all'); startLod(); })
        .catch(function(e) { console.error('[3D Crops]', e); document.getElementById('cropCount').textContent = 'Error: ' + e.message; document.getElementById('cropStats').classList.add('visible'); });

      var btns = document.querySelectorAll('.crop-lyr-btn');
      btns.forEach(function(btn) {
        var t = false;
        btn.addEventListener('touchend', function(e) { e.preventDefault(); t = true; doF(btn); }, {passive: false});
        btn.addEventListener('click', function() { if (t) { t = false; return; } doF(btn); });
      });
      function doF(btn) { if (!allPts) return; btns.forEach(function(b){b.classList.remove('active');}); btn.classList.add('active'); activeFilter = btn.getAttribute('data-crop-type'); render(allPts, activeFilter); }

    } catch (e) { document.getElementById("cropMap3d").innerHTML = "<div style='padding:20px;color:red;'>Init failed: " + e.message + "</div>"; }
  });
})();
</script>