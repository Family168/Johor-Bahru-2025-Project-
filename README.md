<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>全新山 Condo 地图 | 建成与未建成项目</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.css" />
  <style>
    #map { height: 100vh; }
    .popup-content img {
      width: 100%;
      height: auto;
      margin-bottom: 8px;
      border-radius: 8px;
    }
    .popup-content h3 {
      margin: 0 0 5px;
      font-size: 16px;
    }
    .popup-content p {
      margin: 0;
      font-size: 14px;
      color: #555;
    }
    #sidebar {
      position: absolute;
      top: 10px;
      left: 10px;
      background: rgba(255, 255, 255, 0.8);
      padding: 10px;
      border-radius: 8px;
      max-height: 90%;
      overflow-y: auto;
      width: 300px;
      z-index: 1000;
    }
    #sidebar h3 {
      font-size: 18px;
      margin-bottom: 10px;
    }
    #sidebar ul {
      list-style-type: none;
      padding: 0;
    }
    #sidebar li {
      padding: 5px;
      margin-bottom: 8px;
      background-color: #f0f0f0;
      border-radius: 5px;
      cursor: pointer;
    }
    #sidebar li:hover {
      background-color: #e0e0e0;
    }
  </style>
</head>
<body>

<div id="map"></div>
<div id="sidebar">
  <h3>楼盘列表</h3>
  <ul id="condo-list"></ul>
</div>

<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
<script src="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.js"></script>
<script>
  // 初始化地图（白色风格）
  var map = L.map('map').setView([1.4927, 103.7414], 12);

  L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; OpenStreetMap contributors & CartoDB',
    maxZoom: 18
  }).addTo(map);

  // Condo 数据
  var condos = [
    {
      name: "R&F Princess Cove",
      status: "已建成",
      coords: [1.4580, 103.7642],
      image: "https://example.com/rf.jpg",
      desc: "豪华海景公寓，邻近新加坡关卡。",
    },
    {
      name: "The Astaka @ 1Bukit Senyum",
      status: "已建成",
      coords: [1.4650, 103.7580],
      image: "https://example.com/astaka.jpg",
      desc: "全新山最高住宅楼，超五星级设施。",
    },
    {
      name: "Coronation Square",
      status: "未建成",
      coords: [1.4637, 103.7579],
      image: "https://example.com/coronation.jpg",
      desc: "未来综合发展，包含商场、办公室、酒店。",
    },
    {
      name: "TriTower Residence",
      status: "已建成",
      coords: [1.4628, 103.7591],
      image: "https://example.com/tritower.jpg",
      desc: "步行可达CIQ关卡，适合通勤族。",
    },
    {
      name: "Sky88 @ Bukit Senyum",
      status: "未建成",
      coords: [1.4670, 103.7595],
      image: "https://example.com/sky88.jpg",
      desc: "新地标项目，未来地铁站旁。",
    },
    {
      name: "Paragon Suites",
      status: "已建成",
      coords: [1.4624, 103.7605],
      image: "https://example.com/paragon.jpg",
      desc: "现代化高档公寓，配套完善。",
    },
    {
      name: "Suasana Iskandar",
      status: "已建成",
      coords: [1.4620, 103.7597],
      image: "https://example.com/suasana.jpg",
      desc: "坐落于新加坡关卡附近，便捷通行。",
    },
    {
      name: "The Straits View Residences",
      status: "已建成",
      coords: [1.4703, 103.7565],
      image: "https://example.com/straitsview.jpg",
      desc: "海景公寓，临近购物中心。",
    },
    {
      name: "Danga Bay Country Garden",
      status: "已建成",
      coords: [1.4756, 103.7230],
      image: "https://example.com/danga.jpg",
      desc: "滨海高档住宅，设施齐全。",
    },
    {
      name: "Tebrau City Residences",
      status: "未建成",
      coords: [1.5515, 103.7855],
      image: "https://example.com/tebrau.jpg",
      desc: "未来大型商业区一部分，临近大型购物中心。",
    }
  ];

  // 在地图上添加标记
  var sidebar = document.getElementById('condo-list');

  condos.forEach(function(condo, index) {
    var color = condo.status === "已建成" ? "blue" : "red";
    var marker = L.circleMarker(condo.coords, {
      radius: 8,
      color: color,
      fillColor: color,
      fillOpacity: 0.8
    }).addTo(map);

    var popupContent = `
      <div class="popup-content">
        <img src="${condo.image}" alt="${condo.name}">
        <h3>${condo.name}</h3>
        <p>状态：${condo.status}</p>
        <p>${condo.desc}</p>
      </div>
    `;

    marker.bindPopup(popupContent);

    // 在侧边栏添加楼盘列表
    var listItem = document.createElement('li');
    listItem.textContent = condo.name;
    listItem.onclick = function() {
      map.setView(condo.coords, 16);
      marker.openPopup();
    };
    sidebar.appendChild(listItem);
  });

  // 搜索功能
  L.Control.geocoder({
    defaultMarkGeocode: false,
    placeholder: '搜索楼盘名称...',
    geocoder: L.Control.Geocoder.nominatim(),
    collapsed: false
  })
  .on('markgeocode', function(e) {
    map.setView(e.geocode.center, 16);
  })
  .addTo(map);

</script>

</body>
</html>
