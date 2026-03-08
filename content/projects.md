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
<div class="cesium-container">
<link href="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Widgets/widgets.css" rel="stylesheet">
<script src="https://cesium.com/downloads/cesiumjs/releases/1.114/Build/Cesium/Cesium.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>

<div id="map3d" style="display: block; width: 100%; height: 600px; margin-top: 20px; border-radius: 4px; position: relative; z-index: 1; background-color: #1c1c1c;"></div>
</div>

<!-- 调试面板 -->
<div id="debug-log" style="margin-top: 10px; padding: 10px; background: #fffbe6; border: 1px solid #ffe58f; border-radius: 6px; font-size: 13px; color: #333; max-height: 150px; overflow-y: auto;">调试信息：等待操作...</div>

<!-- 控制面板 -->
<div id="layer-panel" style="position: relative; z-index: 99999; margin-top: 15px; padding: 15px; background: #ffffff; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); border: 1px solid #e9ecef;">
  <strong style="display: block; margin-bottom: 12px; font-size: 16px;">3D Layers Control:</strong>
  <div style="display: flex; flex-wrap: wrap; gap: 10px; font-size: 16px; color: #333;">
    <button type="button" class="lyr-btn" data-layer="/data/NDVI.KMZ" style="display: flex; align-items: center; padding: 10px 16px; border: 2px solid #ccc; border-radius: 6px; cursor: pointer; background: #fff; font-size: 16px; color: #333; -webkit-appearance: none; appearance: none; touch-action: manipulation;">
      <span class="lyr-icon" style="display: inline-block; width: 20px; height: 20px; border: 2px solid #999; border-radius: 4px; margin-right: 8px; text-align: center; line-height: 20px; font-size: 14px; flex-shrink: 0;"></span>
      <span>NDVI</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/lcc.kmz" style="display: flex; align-items: center; padding: 10px 16px; border: 2px solid #ccc; border-radius: 6px; cursor: pointer; background: #fff; font-size: 16px; color: #333; -webkit-appearance: none; appearance: none; touch-action: manipulation;">
      <span class="lyr-icon" style="display: inline-block; width: 20px; height: 20px; border: 2px solid #999; border-radius: 4px; margin-right: 8px; text-align: center; line-height: 20px; font-size: 14px; flex-shrink: 0;"></span>
      <span>LCC</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/maize.kmz" style="display: flex; align-items: center; padding: 10px 16px; border: 2px solid #ccc; border-radius: 6px; cursor: pointer; background: #fff; font-size: 16px; color: #333; -webkit-appearance: none; appearance: none; touch-action: manipulation;">
      <span class="lyr-icon" style="display: inline-block; width: 20px; height: 20px; border: 2px solid #999; border-radius: 4px; margin-right: 8px; text-align: center; line-height: 20px; font-size: 14px; flex-shrink: 0;"></span>
      <span>Corn</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/soybean.kmz" style="display: flex; align-items: center; padding: 10px 16px; border: 2px solid #ccc; border-radius: 6px; cursor: pointer; background: #fff; font-size: 16px; color: #333; -webkit-appearance: none; appearance: none; touch-action: manipulation;">
      <span class="lyr-icon" style="display: inline-block; width: 20px; height: 20px; border: 2px solid #999; border-radius: 4px; margin-right: 8px; text-align: center; line-height: 20px; font-size: 14px; flex-shrink: 0;"></span>
      <span>Soybean</span>
    </button>
    <button type="button" class="lyr-btn" data-layer="/data/rice.kmz" style="display: flex; align-items: center; padding: 10px 16px; border: 2px solid #ccc; border-radius: 6px; cursor: pointer; background: #fff; font-size: 16px; color: #333; -webkit-appearance: none; appearance: none; touch-action: manipulation;">
      <span class="lyr-icon" style="display: inline-block; width: 20px; height: 20px; border: 2px solid #999; border-radius: 4px; margin-right: 8px; text-align: center; line-height: 20px; font-size: 14px; flex-shrink: 0;"></span>
      <span>Rice</span>
    </button>
  </div>
</div>

<script>
function debugLog(msg) {
  var el = document.getElementById('debug-log');
  if (el) {
    el.innerHTML += '<br>' + new Date().toLocaleTimeString() + ' - ' + msg;
    el.scrollTop = el.scrollHeight;
  }
}

function getMimeType(filename) {
  var ext = filename.split('.').pop().toLowerCase();
  var map = {
    'png': 'image/png',
    'jpg': 'image/jpeg',
    'jpeg': 'image/jpeg',
    'gif': 'image/gif',
    'bmp': 'image/bmp',
    'tif': 'image/tiff',
    'tiff': 'image/tiff',
    'webp': 'image/webp',
    'svg': 'image/svg+xml'
  };
  return map[ext] || 'application/octet-stream';
}

function loadKmzManually(viewer, kmzUrl) {
  debugLog('下载: ' + kmzUrl);
  return fetch(kmzUrl)
    .then(function(response) {
      if (!response.ok) throw new Error('HTTP ' + response.status);
      debugLog('下载完成，解压中...');
      return response.arrayBuffer();
    })
    .then(function(arrayBuffer) {
      return JSZip.loadAsync(arrayBuffer);
    })
    .then(function(zip) {
      debugLog('解压成功，处理文件...');

      var kmlFile = null;
      var kmlFileName = '';
      var imageFiles = {};

      // 分类：找出 KML 和所有图片
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
      debugLog('KML: ' + kmlFileName + ', 图片: ' + Object.keys(imageFiles).length + ' 个');

      // 先把所有图片转成 Blob URL
      var imagePromises = [];
      var imageUrlMap = {};

      Object.keys(imageFiles).forEach(function(imgPath) {
        var promise = imageFiles[imgPath].async('blob').then(function(blob) {
          var mime = getMimeType(imgPath);
          var typedBlob = new Blob([blob], {type: mime});
          var blobUrl = URL.createObjectURL(typedBlob);
          imageUrlMap[imgPath] = blobUrl;
          debugLog('图片转换: ' + imgPath);
        });
        imagePromises.push(promise);
      });

      return Promise.all(imagePromises).then(function() {
        return kmlFile.async('string').then(function(kmlString) {
          // 替换 KML 中所有图片引用为 Blob URL
          Object.keys(imageUrlMap).forEach(function(imgPath) {
            // 替换各种可能的引用方式
            var escaped = imgPath.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
            var regex = new RegExp(escaped, 'g');
            kmlString = kmlString.replace(regex, imageUrlMap[imgPath]);

            // 也处理只用文件名引用的情况
            var baseName = imgPath.split('/').pop();
            if (baseName !== imgPath) {
              var escapedBase = baseName.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
              var regexBase = new RegExp(escapedBase, 'g');
              kmlString = kmlString.replace(regexBase, imageUrlMap[imgPath]);
            }
          });

          debugLog('KML 图片路径替换完成，加载到 Cesium...');
          var kmlBlob = new Blob([kmlString], {type: 'application/vnd.google-earth.kml+xml'});
          var kmlBlobUrl = URL.createObjectURL(kmlBlob);

          return viewer.dataSources.add(
            Cesium.KmlDataSource.load(kmlBlobUrl, {
              camera: viewer.scene.camera,
              canvas: viewer.scene.canvas,
              clampToGround: true
            })
          ).then(function(dataSource) {
            // 保存 blob URL 引用以便后续清理
            dataSource._blobUrls = [kmlBlobUrl].concat(Object.values(imageUrlMap));
            debugLog('图层渲染成功!');
            return dataSource;
          });
        });
      });
    });
}

document.addEventListener("DOMContentLoaded", function() {
  debugLog('页面加载完成');

  try {
    Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJiNGU2MzgwZS1jNmM0LTQ4MDItOTc1ZS0wMTEyODNmOGNlMTYiLCJpZCI6NDAwMDcwLCJpYXQiOjE3NzI5Mzg2MDJ9.JTTgTyuiRGuJKpLArTT6KoAkzkC4TaB_M_FiOtWPwcU';
    var viewer = new Cesium.Viewer("map3d", {
      terrain: Cesium.Terrain.fromWorldTerrain(),
      baseLayerPicker: false,
      geocoder: false,
      animation: false,
      timeline: false,
      navigationHelpButton: false
    });
    debugLog('Cesium 初始化成功');

    window.loaded3DLayers = {};
    var layerStates = {};
    var loadingStates = {};

    function toggleLayer(btn) {
      var kmlUrl = btn.getAttribute('data-layer');
      var icon = btn.querySelector('.lyr-icon');
      var isActive = layerStates[kmlUrl] || false;

      // 防止重复点击
      if (loadingStates[kmlUrl]) {
        debugLog('图层正在加载中，请等待...');
        return;
      }

      if (!isActive) {
        layerStates[kmlUrl] = true;
        loadingStates[kmlUrl] = true;
        btn.style.borderColor = '#ff9800';
        btn.style.background = '#fff3e0';
        icon.textContent = '⏳';
        icon.style.borderColor = '#ff9800';
        icon.style.color = '#ff9800';

        var isKmz = kmlUrl.toLowerCase().endsWith('.kmz');
        var loadPromise;

        if (isKmz) {
          loadPromise = loadKmzManually(viewer, kmlUrl);
        } else {
          loadPromise = viewer.dataSources.add(
            Cesium.KmlDataSource.load(kmlUrl, {
              camera: viewer.scene.camera,
              canvas: viewer.scene.canvas,
              clampToGround: true
            })
          );
        }

        loadPromise.then(function(dataSource) {
          window.loaded3DLayers[kmlUrl] = dataSource;
          loadingStates[kmlUrl] = false;
          btn.style.borderColor = '#2196F3';
          btn.style.background = '#e3f2fd';
          icon.textContent = '✓';
          icon.style.borderColor = '#2196F3';
          icon.style.color = '#2196F3';
          debugLog('图层加载成功: ' + kmlUrl);
        }).catch(function(error) {
          debugLog('图层加载失败: ' + kmlUrl + ' 错误: ' + error.message);
          layerStates[kmlUrl] = false;
          loadingStates[kmlUrl] = false;
          btn.style.borderColor = '#ccc';
          btn.style.background = '#fff';
          icon.textContent = '';
          icon.style.borderColor = '#999';
        });
      } else {
        layerStates[kmlUrl] = false;
        btn.style.borderColor = '#ccc';
        btn.style.background = '#fff';
        icon.textContent = '';
        icon.style.borderColor = '#999';

        var activeLayer = window.loaded3DLayers[kmlUrl];
        if (activeLayer) {
          // 清理 Blob URL
          if (activeLayer._blobUrls) {
            activeLayer._blobUrls.forEach(function(url) {
              URL.revokeObjectURL(url);
            });
          }
          viewer.dataSources.remove(activeLayer);
          delete window.loaded3DLayers[kmlUrl];
          debugLog('图层已移除: ' + kmlUrl);
        }
      }
    }

    var buttons = document.querySelectorAll('.lyr-btn');
    debugLog('找到 ' + buttons.length + ' 个按钮');

    buttons.forEach(function(btn) {
      var touched = false;

      btn.addEventListener('touchend', function(e) {
        e.preventDefault();
        touched = true;
        toggleLayer(btn);
      }, {passive: false});

      btn.addEventListener('click', function(e) {
        if (touched) {
          touched = false;
          return;
        }
        toggleLayer(this);
      });
    });

    debugLog('所有事件绑定完成');

  } catch (error) {
    debugLog('初始化错误: ' + error.message);
    document.getElementById("map3d").innerHTML =
      "<div style='padding: 20px; color: red;'>3D地球初始化失败：" + error.message + "</div>";
  }
});
</script>