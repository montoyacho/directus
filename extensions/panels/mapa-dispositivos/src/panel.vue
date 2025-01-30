<template>
  <div id="map" style="width: 100%; height: 100%;"></div>
</template>

<script setup>
import { onMounted } from 'vue';
import 'leaflet/dist/leaflet.css';
import L from 'leaflet';

onMounted(() => {
  const map = L.map('map').setView([10.77763, -73.01998], 10);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  // Cargar datos desde Directus
  fetch('/items/dispositivos')
    .then(response => response.json())
    .then(data => {
      data.data.forEach(device => {
        const { coordinates } = device.ubicacion;
        if (coordinates) {
          L.marker([coordinates[1], coordinates[0]]).addTo(map)
            .bindPopup(`<b>${device.nombre}</b><br>${device.descripcion}`);
        }
      });
    })
    .catch(error => console.error('Error al cargar dispositivos:', error));
});
</script>
