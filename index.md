---
layout: default
title: "德國旅遊互動地圖"
---

<div id="map" style="height: 600px;"></div>

<script>
    window.addEventListener('load', function() {
        const map = L.map('map').setView([51.1657, 10.4515], 6);
        const baseUrl = "{{ site.baseurl }}";

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

        fetch(`${baseUrl}/assets/germany.geojson`)
            .then(res => res.json())
            .then(data => {
                L.geoJSON(data, {
                    style: { color: "#3388ff", weight: 2, fillOpacity: 0.1 },
                    onEachFeature: (feature, layer) => {
                        layer.on({
                            mouseover: (e) => e.target.setStyle({ fillOpacity: 0.4 }),
                            mouseout: (e) => e.target.setStyle({ fillOpacity: 0.1 }),
                            click: () => {
                                // 這裡統一轉小寫作為跳轉路徑
                                const id = feature.properties.name.toLowerCase();
                                window.location.href = `${baseUrl}/${id}/`;
                            }
                        });
                        layer.bindTooltip(feature.properties.name);
                    }
                }).addTo(map);
            });
    });
</script>
