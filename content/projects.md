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
.crop-stats { display: flex; gap: 16px; margin-top: 12px; padding-top: 12px; border-top: 1px solid rgba(128,128,128,0.1); font-size: 12px; opacity: 0.35; transition: opacity 0.3s; flex-wrap: wrap; }
.crop-stats.visible { opacity: 0.6; }
.crop-stats span { display: flex; align-items: center; gap: 6px; }
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

<div class="crop-canvas-wrap" id="cropCanvasWrap">
  <div class="crop-loading" id="cropLoading">Loading vegetation data…</div>
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
    <span>🌱 <strong id="cropCount">—</strong> plants</span>
    <span>📊 MODIS NDVI + NASA Blue Marble</span>
  </div>
</div>
</div>

<script>
(function() {
  'use strict';
  var TYPES = ['tropical','forest','crop','grass'];

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
  var earthGeo = new THREE.SphereGeometry(1, 72, 54);
  var earthMat = new THREE.MeshPhongMaterial({ color: 0xffffff, shininess: 15, specular: 0x111122 });
  globeGroup.add(new THREE.Mesh(earthGeo, earthMat));

  // Load earth texture with auto-downsize for mobile (iOS max = 4096)
  var earthImg = new Image();
  earthImg.crossOrigin = 'anonymous';
  earthImg.onload = function() {
    var maxSize = Math.min(renderer.capabilities.maxTextureSize, 4096);
    var w = earthImg.width, h = earthImg.height;
    if (w > maxSize || h > maxSize) {
      var ratio = Math.min(maxSize / w, maxSize / h);
      var nw = Math.floor(w * ratio), nh = Math.floor(h * ratio);
      var cv = document.createElement('canvas'); cv.width = nw; cv.height = nh;
      cv.getContext('2d').drawImage(earthImg, 0, 0, nw, nh);
      var tex = new THREE.CanvasTexture(cv);
      console.log('[Earth] Resized ' + w + 'x' + h + ' → ' + nw + 'x' + nh);
    } else {
      var tex = new THREE.Texture(earthImg);
    }
    tex.anisotropy = renderer.capabilities.getMaxAnisotropy();
    tex.needsUpdate = true;
    earthMat.map = tex; earthMat.needsUpdate = true;
  };
  earthImg.src = '/images/earth.jpg';

  // Atmosphere
  var ag = new THREE.SphereGeometry(1.015,48,36);
  globeGroup.add(new THREE.Mesh(ag, new THREE.MeshPhongMaterial({ color:0x6699cc, transparent:true, opacity:0.06 })));
  var gg = new THREE.SphereGeometry(1.06,32,24);
  globeGroup.add(new THREE.Mesh(gg, new THREE.MeshBasicMaterial({ color:0x3366aa, transparent:true, opacity:0.045, side:THREE.BackSide })));

  // Stars
  var sp=[]; for(var i=0;i<2000;i++){var r=20+Math.random()*50,th=Math.random()*Math.PI*2,ph=Math.acos(2*Math.random()-1);sp.push(r*Math.sin(ph)*Math.cos(th),r*Math.sin(ph)*Math.sin(th),r*Math.cos(ph));}
  var sg=new THREE.BufferGeometry(); sg.setAttribute('position',new THREE.Float32BufferAttribute(sp,3));
  scene.add(new THREE.Points(sg, new THREE.PointsMaterial({color:0xffffff,size:0.08,transparent:true,opacity:0.7})));

  function ll2v(lat,lon,r){var p=(90-lat)*Math.PI/180,t=(lon+180)*Math.PI/180;return new THREE.Vector3(-r*Math.sin(p)*Math.cos(t),r*Math.cos(p),r*Math.sin(p)*Math.sin(t));}

  /* ═══════════════════════════════════════════
     Canvas plant texture generators (transparent PNG-style)
     ★ To use real images later, replace the textures[] arrays
       with: new THREE.TextureLoader().load('/images/tree1.png')
     ═══════════════════════════════════════════ */
  function drawBroadleafTree(sz, trunkCol, leafCol1, leafCol2) {
    var c = document.createElement('canvas'); c.width = sz; c.height = sz * 1.4;
    var x = c.getContext('2d');
    // Trunk
    x.fillStyle = trunkCol;
    x.beginPath(); x.moveTo(sz*0.42, c.height); x.lineTo(sz*0.46, c.height*0.5);
    x.lineTo(sz*0.54, c.height*0.5); x.lineTo(sz*0.58, c.height); x.fill();
    // Branches
    x.strokeStyle = trunkCol; x.lineWidth = 2;
    x.beginPath(); x.moveTo(sz*0.48, c.height*0.55); x.quadraticCurveTo(sz*0.3, c.height*0.45, sz*0.22, c.height*0.35); x.stroke();
    x.beginPath(); x.moveTo(sz*0.52, c.height*0.5); x.quadraticCurveTo(sz*0.7, c.height*0.4, sz*0.78, c.height*0.3); x.stroke();
    // Leaf clusters (multiple overlapping circles with variation)
    var clusters = [[0.5,0.32,0.32],[0.3,0.35,0.24],[0.7,0.3,0.22],[0.4,0.2,0.2],[0.6,0.18,0.18],[0.35,0.48,0.18],[0.65,0.45,0.16],[0.5,0.12,0.15],[0.25,0.22,0.16],[0.75,0.2,0.14]];
    for (var i = 0; i < clusters.length; i++) {
      var cl = clusters[i];
      x.fillStyle = i % 2 === 0 ? leafCol1 : leafCol2;
      x.globalAlpha = 0.75 + Math.random() * 0.25;
      x.beginPath(); x.arc(sz * cl[0], c.height * cl[1], sz * cl[2], 0, Math.PI * 2); x.fill();
    }
    // Highlights
    x.globalAlpha = 0.15; x.fillStyle = '#fff';
    x.beginPath(); x.arc(sz * 0.4, c.height * 0.22, sz * 0.12, 0, Math.PI * 2); x.fill();
    x.globalAlpha = 1;
    return new THREE.CanvasTexture(c);
  }

  function drawPineTree(sz, trunkCol, leafCol1, leafCol2) {
    var c = document.createElement('canvas'); c.width = sz; c.height = sz * 1.6;
    var x = c.getContext('2d');
    x.fillStyle = trunkCol;
    x.fillRect(sz * 0.45, c.height * 0.7, sz * 0.1, c.height * 0.3);
    // Tiered triangle layers
    var tiers = [[0.65,0.25,0.7],[0.55,0.15,0.55],[0.45,0.05,0.42],[0.3,0.0,0.28]];
    for (var t = 0; t < tiers.length; t++) {
      x.fillStyle = t % 2 === 0 ? leafCol1 : leafCol2;
      x.globalAlpha = 0.85 + t * 0.04;
      var w = tiers[t][0], top = tiers[t][1], h = tiers[t][2];
      x.beginPath();
      x.moveTo(sz * 0.5, c.height * top);
      x.lineTo(sz * (0.5 - w/2), c.height * (top + h));
      x.lineTo(sz * (0.5 + w/2), c.height * (top + h));
      x.closePath(); x.fill();
    }
    x.globalAlpha = 0.1; x.fillStyle = '#fff';
    x.beginPath(); x.moveTo(sz*0.5,c.height*0.02); x.lineTo(sz*0.35,c.height*0.4); x.lineTo(sz*0.5,c.height*0.3); x.fill();
    x.globalAlpha = 1;
    return new THREE.CanvasTexture(c);
  }

  function drawFlower(sz, stemCol, petalCol, centerCol) {
    var c = document.createElement('canvas'); c.width = sz; c.height = sz * 1.3;
    var x = c.getContext('2d');
    // Stem
    x.strokeStyle = stemCol; x.lineWidth = sz * 0.03;
    x.beginPath(); x.moveTo(sz*0.5, c.height); x.quadraticCurveTo(sz*0.48, c.height*0.5, sz*0.5, c.height*0.35); x.stroke();
    // Leaves on stem
    x.fillStyle = stemCol; x.globalAlpha = 0.8;
    x.beginPath(); x.ellipse(sz*0.38, c.height*0.65, sz*0.1, sz*0.04, -0.4, 0, Math.PI*2); x.fill();
    x.beginPath(); x.ellipse(sz*0.6, c.height*0.55, sz*0.08, sz*0.035, 0.3, 0, Math.PI*2); x.fill();
    // Petals
    x.globalAlpha = 0.9;
    var cx = sz*0.5, cy = c.height*0.28, pr = sz*0.14;
    for (var p = 0; p < 6; p++) {
      var a = (p/6)*Math.PI*2 - Math.PI/2;
      x.fillStyle = petalCol;
      x.beginPath(); x.ellipse(cx + Math.cos(a)*pr, cy + Math.sin(a)*pr, pr*0.8, pr*0.5, a, 0, Math.PI*2); x.fill();
    }
    // Center
    x.globalAlpha = 1; x.fillStyle = centerCol;
    x.beginPath(); x.arc(cx, cy, sz*0.06, 0, Math.PI*2); x.fill();
    return new THREE.CanvasTexture(c);
  }

  function drawBush(sz, col1, col2, col3) {
    var c = document.createElement('canvas'); c.width = sz; c.height = sz * 0.8;
    var x = c.getContext('2d');
    var blobs = [[0.25,0.55,0.22],[0.5,0.4,0.3],[0.75,0.55,0.2],[0.35,0.35,0.18],[0.65,0.35,0.16],[0.5,0.25,0.2],[0.4,0.6,0.15],[0.6,0.6,0.15]];
    var cols = [col1, col2, col3];
    for (var i = 0; i < blobs.length; i++) {
      x.fillStyle = cols[i % 3]; x.globalAlpha = 0.7 + Math.random() * 0.3;
      x.beginPath(); x.arc(sz * blobs[i][0], c.height * blobs[i][1], sz * blobs[i][2], 0, Math.PI * 2); x.fill();
    }
    x.globalAlpha = 0.12; x.fillStyle = '#000';
    x.beginPath(); x.ellipse(sz*0.5, c.height*0.85, sz*0.35, c.height*0.08, 0, 0, Math.PI*2); x.fill();
    x.globalAlpha = 1;
    return new THREE.CanvasTexture(c);
  }

  function drawWheat(sz, stalkCol, headCol) {
    var c = document.createElement('canvas'); c.width = sz; c.height = sz * 1.8;
    var x = c.getContext('2d');
    // Multiple stalks
    var stalks = [0.3, 0.45, 0.6, 0.75];
    for (var s = 0; s < stalks.length; s++) {
      var sx = sz * stalks[s], sway = Math.sin(s * 2.1) * sz * 0.04;
      x.strokeStyle = stalkCol; x.lineWidth = 1.5;
      x.beginPath(); x.moveTo(sx, c.height);
      x.quadraticCurveTo(sx + sway, c.height * 0.5, sx + sway * 0.6, c.height * 0.18); x.stroke();
      // Head
      x.fillStyle = headCol; x.globalAlpha = 0.85;
      x.beginPath(); x.ellipse(sx + sway * 0.6, c.height * 0.14, sz * 0.04, sz * 0.11, 0.15 * s, 0, Math.PI * 2); x.fill();
      // Awns
      x.strokeStyle = headCol; x.lineWidth = 0.7; x.globalAlpha = 0.6;
      for (var a = 0; a < 3; a++) {
        var ay = c.height * 0.1 + a * sz * 0.04;
        x.beginPath(); x.moveTo(sx + sway*0.6, ay); x.lineTo(sx + sway*0.6 + sz*0.06, ay - sz*0.03); x.stroke();
        x.beginPath(); x.moveTo(sx + sway*0.6, ay); x.lineTo(sx + sway*0.6 - sz*0.06, ay - sz*0.03); x.stroke();
      }
    }
    x.globalAlpha = 1;
    return new THREE.CanvasTexture(c);
  }

  function drawPalm(sz, trunkCol, leafCol) {
    var c = document.createElement('canvas'); c.width = sz; c.height = sz * 1.5;
    var x = c.getContext('2d');
    // Curved trunk
    x.strokeStyle = trunkCol; x.lineWidth = sz * 0.05;
    x.beginPath(); x.moveTo(sz*0.5, c.height);
    x.quadraticCurveTo(sz*0.55, c.height*0.5, sz*0.48, c.height*0.3); x.stroke();
    // Fronds
    x.strokeStyle = leafCol; x.lineWidth = 2;
    x.fillStyle = leafCol;
    var fronds = [[-0.7,0.1],[-0.4,-0.15],[0.0,-0.2],[0.4,-0.1],[0.7,0.15],[-0.5,0.2],[0.5,0.25]];
    for (var f = 0; f < fronds.length; f++) {
      var fx = sz*0.48 + sz*fronds[f][0]*0.45, fy = c.height*0.3 + c.height*fronds[f][1];
      x.globalAlpha = 0.7 + Math.random()*0.3;
      x.beginPath(); x.moveTo(sz*0.48, c.height*0.3);
      x.quadraticCurveTo((sz*0.48+fx)/2, fy - sz*0.1, fx, fy); x.stroke();
      // Leaf shape
      x.beginPath(); x.ellipse((sz*0.48+fx)/2, (c.height*0.3+fy)/2 - sz*0.02, sz*0.15, sz*0.04, Math.atan2(fy-c.height*0.3, fx-sz*0.48), 0, Math.PI*2); x.fill();
    }
    x.globalAlpha = 1;
    return new THREE.CanvasTexture(c);
  }

  // Generate texture variations for each type
  // ★ REPLACE THESE WITH real PNGs later:
  //   textures.tropical = [new THREE.TextureLoader().load('/images/tree1.png'), ...]
  var SZ = 128;
  var textures = {
    tropical: [
      drawBroadleafTree(SZ, '#5D4037', '#1a6e1a', '#228B22'),
      drawBroadleafTree(SZ, '#4E342E', '#0d5e0d', '#1b8a1b'),
      drawBroadleafTree(SZ, '#6D4C41', '#176b17', '#2d8a2d'),
      drawPalm(SZ, '#795548', '#1e7a1e'),
      drawBroadleafTree(SZ, '#5D4037', '#145a14', '#1f8522'),
    ],
    forest: [
      drawPineTree(SZ, '#6D4C41', '#2e7d32', '#1b5e20'),
      drawPineTree(SZ, '#5D4037', '#388e3c', '#256d28'),
      drawPineTree(SZ, '#4E342E', '#43a047', '#2e7d32'),
      drawBroadleafTree(SZ, '#5D4037', '#388e3c', '#4caf50'),
      drawPineTree(SZ, '#6D4C41', '#1b5e20', '#358538'),
    ],
    crop: [
      drawWheat(SZ, '#8d6e27', '#c0ca33'),
      drawWheat(SZ, '#9e7e30', '#aed581'),
      drawWheat(SZ, '#7a6020', '#8bc34a'),
      drawFlower(SZ, '#558b2f', '#ffeb3b', '#ff9800'),
      drawWheat(SZ, '#8d6e27', '#9ccc65'),
    ],
    grass: [
      drawBush(SZ, '#66bb6a', '#81c784', '#a5d6a7'),
      drawBush(SZ, '#4caf50', '#73c778', '#98cb9c'),
      drawFlower(SZ, '#4a7c23', '#ff6b8a', '#ffeb3b'),
      drawFlower(SZ, '#558b2f', '#ce93d8', '#fff59d'),
      drawBush(SZ, '#81c784', '#a5d6a7', '#c5e1a5'),
      drawFlower(SZ, '#4a7c23', '#ff4081', '#ffeb3b'),
      drawFlower(SZ, '#558b2f', '#448aff', '#fff9c4'),
    ]
  };

  // Set texture filtering
  Object.keys(textures).forEach(function(k) {
    textures[k].forEach(function(t) {
      t.minFilter = THREE.LinearFilter;
      t.magFilter = THREE.LinearFilter;
    });
  });

  var CFG = {
    tropical: { sc: 0.035, aspect: 1.4 },
    forest:   { sc: 0.030, aspect: 1.6 },
    crop:     { sc: 0.020, aspect: 1.8 },
    grass:    { sc: 0.016, aspect: 0.8 }
  };

  // Cross-billboard geometry: two perpendicular planes forming an X
  // Visible from all camera angles, no need for per-frame billboard updates
  function makeCrossGeo() {
    var p1 = new THREE.PlaneGeometry(1, 1);
    var p2 = new THREE.PlaneGeometry(1, 1);
    p2.rotateY(Math.PI / 2);
    // Merge into single geometry
    var pos1 = p1.attributes.position.array, pos2 = p2.attributes.position.array;
    var nrm1 = p1.attributes.normal.array, nrm2 = p2.attributes.normal.array;
    var uv1 = p1.attributes.uv.array, uv2 = p2.attributes.uv.array;
    var idx1 = p1.index.array, idx2 = p2.index.array;
    var pos = new Float32Array(pos1.length + pos2.length);
    var nrm = new Float32Array(nrm1.length + nrm2.length);
    var uv = new Float32Array(uv1.length + uv2.length);
    pos.set(pos1, 0); pos.set(pos2, pos1.length);
    nrm.set(nrm1, 0); nrm.set(nrm2, nrm1.length);
    uv.set(uv1, 0); uv.set(uv2, uv1.length);
    var idx = [];
    for (var i = 0; i < idx1.length; i++) idx.push(idx1[i]);
    var off = pos1.length / 3;
    for (var i = 0; i < idx2.length; i++) idx.push(idx2[i] + off);
    var g = new THREE.BufferGeometry();
    g.setAttribute('position', new THREE.BufferAttribute(pos, 3));
    g.setAttribute('normal', new THREE.BufferAttribute(nrm, 3));
    g.setAttribute('uv', new THREE.BufferAttribute(uv, 2));
    g.setIndex(idx);
    return g;
  }

  var crossGeo = makeCrossGeo();

  var meshGroups = {};

  function buildVeg(pts, filter) {
    // Remove old
    Object.keys(meshGroups).forEach(function(k) {
      meshGroups[k].forEach(function(m) { globeGroup.remove(m); });
    });
    meshGroups = {};

    // Group by type
    var grouped = {};
    for (var i = 0; i < pts.length; i++) {
      var p = pts[i], pt = TYPES[p[2]] || 'grass';
      if (filter !== 'all' && pt !== filter) continue;
      if (!grouped[pt]) grouped[pt] = [];
      grouped[pt].push(p);
    }

    var total = 0, dummy = new THREE.Object3D();
    var up = new THREE.Vector3(0, 1, 0);
    var quat = new THREE.Quaternion();
    var mat4 = new THREE.Matrix4();

    Object.keys(grouped).forEach(function(type) {
      var arr = grouped[type], cfg = CFG[type], texArr = textures[type];
      meshGroups[type] = [];

      // Split into sub-groups by texture variant
      var subGroups = {};
      for (var i = 0; i < arr.length; i++) {
        var tIdx = i % texArr.length;
        if (!subGroups[tIdx]) subGroups[tIdx] = [];
        subGroups[tIdx].push(arr[i]);
      }

      Object.keys(subGroups).forEach(function(tIdx) {
        var sub = subGroups[tIdx], n = sub.length;
        var tex = texArr[tIdx];

        var mat = new THREE.MeshBasicMaterial({
          map: tex, transparent: true, alphaTest: 0.05,
          side: THREE.DoubleSide, depthWrite: false
        });

        var mesh = new THREE.InstancedMesh(crossGeo, mat, n);

        for (var i = 0; i < n; i++) {
          var p = sub[i], lat = p[0], lon = p[1], sc = p[4] / 100;
          var s = cfg.sc * (0.5 + sc * 1.0) * (0.7 + ((i * 7) % 100) / 100 * 0.6);

          var pos = ll2v(lat, lon, 1.003);
          var norm = pos.clone().normalize();

          // Build matrix: position on sphere surface, Y-axis = surface normal
          quat.setFromUnitVectors(up, norm);
          mat4.makeRotationFromQuaternion(quat);
          // Offset up by half the height so bottom touches surface
          var halfH = s * cfg.aspect * 0.5;
          var finalPos = pos.clone().add(norm.clone().multiplyScalar(halfH));
          mat4.setPosition(finalPos);
          // Apply scale
          var scaleMat = new THREE.Matrix4().makeScale(s, s * cfg.aspect, s);
          mat4.multiply(scaleMat);

          mesh.setMatrixAt(i, mat4);
        }

        mesh.instanceMatrix.needsUpdate = true;
        globeGroup.add(mesh);
        meshGroups[type].push(mesh);
        total += n;
      });
    });

    document.getElementById('cropCount').textContent = total.toLocaleString();
    document.getElementById('cropStats').classList.add('visible');
  }

  var allPts=null, activeFilter='all';
  fetch('/data/crops.json')
    .then(function(r){if(!r.ok)throw new Error('HTTP '+r.status);return r.json();})
    .then(function(d){allPts=d;document.getElementById('cropLoading').style.display='none';buildVeg(d,'all');})
    .catch(function(e){document.getElementById('cropLoading').textContent='Failed: '+e.message;});

  // Interaction
  var isDrag=false,prev={x:0,y:0},vel={x:0,y:0.003};
  var el=renderer.domElement;
  el.addEventListener('pointerdown',function(e){isDrag=true;prev={x:e.clientX,y:e.clientY};});
  el.addEventListener('pointermove',function(e){if(!isDrag)return;vel.y=(e.clientX-prev.x)*0.004;vel.x=(e.clientY-prev.y)*0.004;prev={x:e.clientX,y:e.clientY};});
  el.addEventListener('pointerup',function(){isDrag=false;});
  el.addEventListener('pointerleave',function(){isDrag=false;});
  el.addEventListener('wheel',function(e){camera.position.z=Math.max(1.3,Math.min(5,camera.position.z+e.deltaY*0.002));},{passive:true});
  var ld=0;
  el.addEventListener('touchstart',function(e){if(e.touches.length===2)ld=Math.hypot(e.touches[0].clientX-e.touches[1].clientX,e.touches[0].clientY-e.touches[1].clientY);},{passive:true});
  el.addEventListener('touchmove',function(e){if(e.touches.length===2){var d=Math.hypot(e.touches[0].clientX-e.touches[1].clientX,e.touches[0].clientY-e.touches[1].clientY);if(ld>0)camera.position.z=Math.max(1.3,Math.min(5,camera.position.z-(d-ld)*0.005));ld=d;}},{passive:true});
  el.addEventListener('touchend',function(){ld=0;},{passive:true});

  function animate(){
    requestAnimationFrame(animate);
    if(!isDrag){vel.y+=(0.0015-vel.y)*0.02;vel.x*=0.95;}
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

  // Filter buttons
  var btns=document.querySelectorAll('.crop-lyr-btn');
  btns.forEach(function(btn){var t=false;btn.addEventListener('touchend',function(e){e.preventDefault();t=true;doF(btn);},{passive:false});btn.addEventListener('click',function(){if(t){t=false;return;}doF(btn);});});
  function doF(btn){if(!allPts)return;btns.forEach(function(b){b.classList.remove('active');});btn.classList.add('active');activeFilter=btn.getAttribute('data-crop-type');buildVeg(allPts,activeFilter);}
})();
</script>