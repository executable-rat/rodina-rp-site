---
layout: page_map
title: Интерактивная карта
permalink: /map/
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

<div class="map-page">
  <div class="map-container-wrapper">
    <div class="map-container">
      <div id="leaflet-map"></div>
      
      <!-- Мобильные фильтры - плавающая панель -->
      <div class="mobile-filters-panel hidden-md-up">
        <div class="mobile-filters-tools">
          <div class="mobile-filters-icons" id="mobileFiltersContainer"></div>
          <button class="mobile-filters-toggle" id="mobileFiltersToggle" title="Все фильтры">
            <i class="fas fa-ellipsis-h"></i>
          </button>
        </div>
        
        <div class="mobile-filters-expanded hidden" id="mobileFiltersExpanded">
          <div class="mobile-filters-grid" id="mobileFiltersGrid"></div>
        </div>
      </div>
      
      <div class="tools-panel">
        <button class="tool-btn" id="toolCoords" title="Определить координаты">
          <i class="fas fa-crosshairs"></i>
        </button>
        <button class="tool-btn" id="toolCenter" title="Вернуться к центру">
          <i class="fas fa-home"></i>
        </button>
        <button class="tool-btn" id="toolFullscreen" title="Полноэкранный режим">
          <i class="fas fa-expand"></i>
        </button>
      </div>
      
      <div class="coords-panel hidden" id="coordsPanel">
        <div class="coords-title">Координаты точки:</div>
        <div class="coords-value" id="coordsValue">x: 0, y: 0</div>
        <button class="copy-btn" id="copyCoords">
          <i class="fas fa-copy"></i> Копировать
        </button>
      </div>
    </div>

    <!-- Десктопные фильтры -->
    <div class="desktop-controls hidden-md-down">
      <div class="controls-title">
        <i class="fas fa-sliders-h"></i>
        <span>Управление картой</span>
      </div>

      <div class="filters-section">
        <div class="filters-grid" id="desktopFiltersContainer"></div>
      </div>
    </div>
  </div>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
const mapConfig = {
  settings: {
    useSimpleCRS: {{ site.data.map_locations.map_settings.use_simple_crs | default: false }},
    imageWidth: {{ site.data.map_locations.map_settings.image_width | default: 2000 }},
    imageHeight: {{ site.data.map_locations.map_settings.image_height | default: 2000 }},
    center: [{{ site.data.map_locations.map_settings.center[0] }}, {{ site.data.map_locations.map_settings.center[1] }}],
    zoom: {{ site.data.map_locations.map_settings.zoom | default: 0 }},
    minZoom: {{ site.data.map_locations.map_settings.min_zoom | default: -2 }},
    maxZoom: {{ site.data.map_locations.map_settings.max_zoom | default: 5 }},
    useCustomImage: {{ site.data.map_locations.map_settings.use_custom_image | default: false }},
    customImageUrl: "{{ site.data.map_locations.map_settings.custom_image_url | default: '' }}",
    customImageBounds: [{{ site.data.map_locations.map_settings.custom_image_bounds[0][0] }}, {{ site.data.map_locations.map_settings.custom_image_bounds[0][1] }}]
  },
  locations: [
    {% for location in site.data.map_locations.locations %}
    {
      id: {{ location.id }},
      title: "{{ location.title | escape }}",
      description: "{{ location.description | escape }}",
      x: {{ location.x }},
      y: {{ location.y }},
      type: "{{ location.type }}",
      icon: "{{ location.icon }}",
      color: "{{ location.color }}",
      {% if location.faction %}
      faction: "{{ location.faction }}",
      {% endif %}
      {% if location.category %}
      category: "{{ location.category }}",
      {% endif %}
      {% if location.danger_level %}
      danger_level: "{{ location.danger_level }}",
      {% endif %}
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ]
};

const filters = [
  { id: 'all', name: 'Все точки', icon: 'globe', color: '#8b0000' },
  { id: 'work', name: 'Работы', icon: 'briefcase', color: '#FFA500' },
  { id: 'government', name: 'Государство', icon: 'landmark', color: '#4169E1' },
  { id: 'shop', name: 'Магазины', icon: 'shopping-cart', color: '#32CD32' },
  { id: 'medical', name: 'Медицина', icon: 'medkit', color: '#FF1493' },
  { id: 'fuel', name: 'Заправки', icon: 'gas-pump', color: '#FFD700' },
  { id: 'fishing', name: 'Рыбалка', icon: 'fish', color: '#00CED1' },
  { id: 'hunting', name: 'Охота', icon: 'crosshairs', color: '#8B4513' },
  { id: 'weapon', name: 'Оружие', icon: 'gun', color: '#DC143C' },
  { id: 'checkpoint', name: 'КПП', icon: 'shield-alt', color: '#4682B4' },
  { id: 'interest', name: 'Интересное', icon: 'star', color: '#FF69B4' },
  { id: 'service', name: 'Сервисы', icon: 'tools', color: '#20B2AA' }
];

const typeNames = {
  work: 'Работа',
  government: 'Государство',
  shop: 'Магазин',
  medical: 'Медицина',
  fuel: 'Заправка',
  fishing: 'Рыбалка',
  hunting: 'Охота',
  weapon: 'Оружие',
  checkpoint: 'КПП',
  interest: 'Интересное',
  service: 'Сервис'
};

const factionNames = {
  mvd: 'МВД',
  fsb: 'ФСБ',
  rosgvardia: 'Росгвардия',
  prosecutor: 'Прокуратура'
};

let map;
let markers = [];
let activeFilter = 'all';
let selectedMarker = null;
let coordsMode = false;
let isFullscreen = false;

document.addEventListener('DOMContentLoaded', function() {
  initializeMap();
  initializeFilters();
  setupEventListeners();
  setupMobileFilters();
});

function initializeMap() {
  if (mapConfig.settings.useSimpleCRS) {
    map = L.map('leaflet-map', {
      crs: L.CRS.Simple,
      minZoom: mapConfig.settings.minZoom,
      maxZoom: mapConfig.settings.maxZoom,
      zoomControl: true,
      attributionControl: false,
      preferCanvas: true
    });

    const southWest = map.unproject([0, mapConfig.settings.imageHeight], map.getMaxZoom() - 1);
    const northEast = map.unproject([mapConfig.settings.imageWidth, 0], map.getMaxZoom() - 1);
    const bounds = new L.LatLngBounds(southWest, northEast);

    if (mapConfig.settings.useCustomImage && mapConfig.settings.customImageUrl) {
      L.imageOverlay(mapConfig.settings.customImageUrl, bounds, {
        opacity: 0.9
      }).addTo(map);
    } else {
      L.rectangle(bounds, {
        color: '#8b0000',
        fillColor: '#1a0a0a',
        fillOpacity: 0.9,
        weight: 3
      }).addTo(map);
      
      for (let x = 0; x <= mapConfig.settings.imageWidth; x += 200) {
        L.polyline([
          map.unproject([x, 0], map.getMaxZoom() - 1),
          map.unproject([x, mapConfig.settings.imageHeight], map.getMaxZoom() - 1)
        ], {
          color: 'rgba(139, 0, 0, 0.2)',
          weight: 1,
          opacity: 0.5
        }).addTo(map);
      }
      
      for (let y = 0; y <= mapConfig.settings.imageHeight; y += 200) {
        L.polyline([
          map.unproject([0, y], map.getMaxZoom() - 1),
          map.unproject([mapConfig.settings.imageWidth, y], map.getMaxZoom() - 1)
        ], {
          color: 'rgba(139, 0, 0, 0.2)',
          weight: 1,
          opacity: 0.5
        }).addTo(map);
      }
    }

    map.fitBounds(bounds);
    map.setMaxBounds(bounds);
    
    const centerPoint = map.unproject([mapConfig.settings.center[0], mapConfig.settings.center[1]], map.getMaxZoom() - 1);
    map.setView(centerPoint, mapConfig.settings.zoom);
  } else {
    map = L.map('leaflet-map', {
      preferCanvas: true
    }).setView(mapConfig.settings.center, mapConfig.settings.zoom);
    
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(map);
  }

  createMarkers();
}

function pixelToLatLng(x, y) {
  if (mapConfig.settings.useSimpleCRS) {
    const maxZoom = map.getMaxZoom() - 1;
    return map.unproject([x, y], maxZoom);
  } else {
    return [y, x];
  }
}

function createMarkers() {
  markers.forEach(marker => map.removeLayer(marker));
  markers = [];

  mapConfig.locations.forEach(location => {
    const latLng = pixelToLatLng(location.x, location.y);
    
    const markerHtml = `
      <div class="custom-marker" style="background-color: ${location.color}; border-color: ${location.color}">
        <i class="fas fa-${location.icon}"></i>
      </div>
    `;

    const customIcon = L.divIcon({
      html: markerHtml,
      iconSize: [40, 40],
      iconAnchor: [20, 20],
      popupAnchor: [0, -20],
      className: 'custom-div-icon'
    });

    const marker = L.marker(latLng, {
      icon: customIcon,
      title: location.title,
      riseOnHover: true
    }).addTo(map);

    marker.bindPopup(`
      <div style="min-width: 250px; max-width: 300px;">
        <h3 style="margin: 0 0 5px 0; color: ${location.color}; border-bottom: 2px solid ${location.color}; padding-bottom: 5px;">
          <i class="fas fa-${location.icon}"></i> ${location.title}
        </h3>
        <p style="margin: 10px 0; color: #000000; font-size: 14px; line-height: 1.4;">${location.description}</p>
        <div style="font-size: 12px; color: #888; margin-top: 10px; padding-top: 10px; border-top: 1px solid #333;">
          <div><i class="fas fa-tag"></i> <strong>Тип:</strong> ${typeNames[location.type] || location.type}</div>
          <div><i class="fas fa-map-marker-alt"></i> <strong>Координаты:</strong> [${location.x}, ${location.y}]</div>
          ${location.faction ? `<div><i class="fas fa-building"></i> <strong>Фракция:</strong> ${factionNames[location.faction] || location.faction}</div>` : ''}
        </div>
      </div>
    `);

    marker.on('click', function(e) {
      e.originalEvent.stopPropagation();
      marker.openPopup();
      
      marker.setIcon(L.divIcon({
        html: `
          <div class="custom-marker" style="background-color: ${location.color}; border-color: ${location.color}; transform: scale(1.2);">
            <i class="fas fa-${location.icon}"></i>
          </div>
        `,
        iconSize: [40, 40],
        iconAnchor: [20, 20],
        popupAnchor: [0, -20],
        className: 'custom-div-icon'
      }));
      
      setTimeout(() => {
        marker.setIcon(customIcon);
      }, 500);
    });

    marker.on('mouseover', function() {
      this.openPopup();
    });

    marker.on('mouseout', function() {
      this.closePopup();
    });

    marker.locationData = location;
    markers.push(marker);
  });
}

function initializeFilters() {
  const desktopContainer = document.getElementById('desktopFiltersContainer');
  const mobileIconsContainer = document.getElementById('mobileFiltersContainer');
  const mobileGridContainer = document.getElementById('mobileFiltersGrid');
  
  if (desktopContainer) {
    desktopContainer.innerHTML = '';
  }
  
  if (mobileIconsContainer) {
    mobileIconsContainer.innerHTML = '';
  }
  
  if (mobileGridContainer) {
    mobileGridContainer.innerHTML = '';
  }
  
  const popularFilters = ['all', 'work', 'government', 'shop', 'medical'];
  
  filters.forEach(filter => {
    // Десктопная версия
    if (desktopContainer) {
      const desktopButton = document.createElement('button');
      desktopButton.className = `filter-btn ${filter.id === 'all' ? 'active' : ''}`;
      desktopButton.dataset.filter = filter.id;
      desktopButton.innerHTML = `
        <i class="fas fa-${filter.icon}" style="color: ${filter.color}"></i>
        <span>${filter.name}</span>
      `;
      
      desktopButton.addEventListener('click', () => {
        setActiveFilter(filter.id);
      });
      
      desktopContainer.appendChild(desktopButton);
    }
    
    // Мобильная версия - популярные иконки
    if (mobileIconsContainer && popularFilters.includes(filter.id)) {
      const mobileIcon = document.createElement('button');
      mobileIcon.className = `mobile-filter-icon-btn ${filter.id === 'all' ? 'active' : ''}`;
      mobileIcon.dataset.filter = filter.id;
      mobileIcon.title = filter.name;
      mobileIcon.innerHTML = `<i class="fas fa-${filter.icon}" style="color: ${filter.color}"></i>`;
      
      mobileIcon.addEventListener('click', () => {
        setActiveFilter(filter.id);
      });
      
      mobileIconsContainer.appendChild(mobileIcon);
    }
    
    // Мобильная версия - все фильтры в развернутом меню
    if (mobileGridContainer) {
      const mobileGridItem = document.createElement('button');
      mobileGridItem.className = `mobile-filter-grid-item ${filter.id === 'all' ? 'active' : ''}`;
      mobileGridItem.dataset.filter = filter.id;
      mobileGridItem.title = filter.name;
      mobileGridItem.innerHTML = `
        <i class="fas fa-${filter.icon}" style="color: ${filter.color}"></i>
        <span class="mobile-filter-name">${filter.name}</span>
      `;
      
      mobileGridItem.addEventListener('click', () => {
        setActiveFilter(filter.id);
        const expandedPanel = document.getElementById('mobileFiltersExpanded');
        if (expandedPanel) {
          expandedPanel.classList.add('hidden');
        }
      });
      
      mobileGridContainer.appendChild(mobileGridItem);
    }
  });
}

function setActiveFilter(filterId) {
  activeFilter = filterId;
  
  // Обновляем десктопные кнопки
  document.querySelectorAll('.filter-btn').forEach(btn => {
    if (btn.dataset.filter === filterId) {
      btn.classList.add('active');
    } else {
      btn.classList.remove('active');
    }
  });
  
  // Обновляем мобильные иконки
  document.querySelectorAll('.mobile-filter-icon-btn').forEach(btn => {
    if (btn.dataset.filter === filterId) {
      btn.classList.add('active');
    } else {
      btn.classList.remove('active');
    }
  });
  
  // Обновляем мобильные grid-элементы
  document.querySelectorAll('.mobile-filter-grid-item').forEach(btn => {
    if (btn.dataset.filter === filterId) {
      btn.classList.add('active');
    } else {
      btn.classList.remove('active');
    }
  });
  
  // Обновляем маркеры на карте
  markers.forEach(marker => {
    if (filterId === 'all' || marker.locationData.type === filterId) {
      if (!map.hasLayer(marker)) {
        map.addLayer(marker);
      }
    } else {
      if (map.hasLayer(marker)) {
        map.removeLayer(marker);
      }
    }
  });
}

function setupMobileFilters() {
  const mobileFiltersToggle = document.getElementById('mobileFiltersToggle');
  const mobileFiltersExpanded = document.getElementById('mobileFiltersExpanded');
  
  if (mobileFiltersToggle && mobileFiltersExpanded) {
    mobileFiltersToggle.addEventListener('click', function(e) {
      e.stopPropagation();
      const isHidden = mobileFiltersExpanded.classList.contains('hidden');
      
      if (isHidden) {
        mobileFiltersExpanded.classList.remove('hidden');
        this.innerHTML = '<i class="fas fa-times"></i>';
        this.title = 'Закрыть фильтры';
      } else {
        mobileFiltersExpanded.classList.add('hidden');
        this.innerHTML = '<i class="fas fa-ellipsis-h"></i>';
        this.title = 'Все фильтры';
      }
    });
    
    // Закрываем мобильное меню при клике вне его
    document.addEventListener('click', function(e) {
      const mobilePanel = document.querySelector('.mobile-filters-panel');
      const expandedPanel = document.getElementById('mobileFiltersExpanded');
      const toggleBtn = document.getElementById('mobileFiltersToggle');
      
      if (mobilePanel && expandedPanel && toggleBtn) {
        if (!mobilePanel.contains(e.target) && !expandedPanel.classList.contains('hidden')) {
          expandedPanel.classList.add('hidden');
          toggleBtn.innerHTML = '<i class="fas fa-ellipsis-h"></i>';
          toggleBtn.title = 'Все фильтры';
        }
      }
    });
  }
}

function setupEventListeners() {
  const toolCoords = document.getElementById('toolCoords');
  if (toolCoords) {
    toolCoords.addEventListener('click', function() {
      coordsMode = !coordsMode;
      this.classList.toggle('active');
      
      if (coordsMode) {
        const coordsPanel = document.getElementById('coordsPanel');
        if (coordsPanel) {
          coordsPanel.classList.remove('hidden');
        }
        map.dragging.disable();
        map.doubleClickZoom.disable();
      } else {
        const coordsPanel = document.getElementById('coordsPanel');
        if (coordsPanel) {
          coordsPanel.classList.add('hidden');
        }
        map.dragging.enable();
        map.doubleClickZoom.enable();
      }
    });
  }
  
  const toolCenter = document.getElementById('toolCenter');
  if (toolCenter) {
    toolCenter.addEventListener('click', function() {
      if (mapConfig.settings.useSimpleCRS) {
        const centerPoint = pixelToLatLng(mapConfig.settings.center[0], mapConfig.settings.center[1]);
        map.setView(centerPoint, mapConfig.settings.zoom);
      } else {
        map.setView(mapConfig.settings.center, mapConfig.settings.zoom);
      }
      
      this.classList.add('active');
      setTimeout(() => this.classList.remove('active'), 300);
    });
  }
  
  const toolFullscreen = document.getElementById('toolFullscreen');
  if (toolFullscreen) {
    toolFullscreen.addEventListener('click', function() {
      const mapContainer = document.querySelector('.map-container');
      
      if (!isFullscreen) {
        if (mapContainer.requestFullscreen) {
          mapContainer.requestFullscreen();
        } else if (mapContainer.webkitRequestFullscreen) {
          mapContainer.webkitRequestFullscreen();
        } else if (mapContainer.msRequestFullscreen) {
          mapContainer.msRequestFullscreen();
        }
      } else {
        if (document.exitFullscreen) {
          document.exitFullscreen();
        } else if (document.webkitExitFullscreen) {
          document.webkitExitFullscreen();
        } else if (document.msExitFullscreen) {
          document.msExitFullscreen();
        }
      }
    });
  }
  
  document.addEventListener('fullscreenchange', handleFullscreenChange);
  document.addEventListener('webkitfullscreenchange', handleFullscreenChange);
  document.addEventListener('msfullscreenchange', handleFullscreenChange);
  
  function handleFullscreenChange() {
    isFullscreen = !!(document.fullscreenElement || 
                      document.webkitFullscreenElement || 
                      document.msFullscreenElement);
    
    const fullscreenBtn = document.getElementById('toolFullscreen');
    if (fullscreenBtn) {
      fullscreenBtn.classList.toggle('active', isFullscreen);
      fullscreenBtn.innerHTML = isFullscreen ? 
        '<i class="fas fa-compress"></i>' : 
        '<i class="fas fa-expand"></i>';
    }
    
    setTimeout(() => {
      map.invalidateSize();
    }, 300);
  }
  
  const copyCoordsBtn = document.getElementById('copyCoords');
  if (copyCoordsBtn) {
    copyCoordsBtn.addEventListener('click', function() {
      const coordsText = document.getElementById('coordsValue').textContent;
      navigator.clipboard.writeText(coordsText).then(() => {
        const originalText = this.innerHTML;
        this.innerHTML = '<i class="fas fa-check"></i> Скопировано!';
        this.style.backgroundColor = '#32CD32';
        
        setTimeout(() => {
          this.innerHTML = originalText;
          this.style.backgroundColor = '';
        }, 2000);
      }).catch(err => {
        const originalText = this.innerHTML;
        this.innerHTML = '<i class="fas fa-times"></i> Ошибка!';
        this.style.backgroundColor = '#DC143C';
        
        setTimeout(() => {
          this.innerHTML = originalText;
          this.style.backgroundColor = '';
        }, 2000);
      });
    });
  }
  
  map.on('click', function(e) {
    if (coordsMode) {
      let x, y;
      
      if (mapConfig.settings.useSimpleCRS) {
        const maxZoom = map.getMaxZoom() - 1;
        const point = map.project(e.latlng, maxZoom);
        x = Math.round(point.x);
        y = Math.round(point.y);
      } else {
        x = e.latlng.lng.toFixed(2);
        y = e.latlng.lat.toFixed(2);
      }
      
      const coordsValue = document.getElementById('coordsValue');
      const coordsPanel = document.getElementById('coordsPanel');
      
      if (coordsValue) {
        coordsValue.textContent = `x: ${x}, y: ${y}`;
      }
      
      if (coordsPanel) {
        coordsPanel.classList.remove('hidden');
      }
      
      const tempMarker = L.marker(e.latlng, {
        icon: L.divIcon({
          html: '<div class="custom-marker new-point" style="background-color: #FFD700; border-color: #FFA500;"><i class="fas fa-crosshairs"></i></div>',
          iconSize: [40, 40],
          iconAnchor: [20, 20],
          className: 'temp-marker'
        }),
        zIndexOffset: 1000
      }).addTo(map);
      
      setTimeout(() => {
        map.removeLayer(tempMarker);
      }, 3000);
      
      map.panTo(e.latlng);
    } else {
      selectedMarker = null;
      
      map.eachLayer(function(layer) {
        if (layer.closePopup) {
          layer.closePopup();
        }
      });
    }
  });
  
  map.on('dblclick', function(e) {
    if (!coordsMode) {
      map.zoomIn();
    }
  });
  
  window.addEventListener('resize', function() {
    setTimeout(() => {
      map.invalidateSize();
    }, 200);
  });
}
</script>