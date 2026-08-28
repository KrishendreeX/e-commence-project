<template>
  <section class="zone-map-section" aria-label="CleanSpaces active zones map">
    <div ref="mapEl" class="zone-map"></div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'
import markerIcon2x from 'leaflet/dist/images/marker-icon-2x.png'
import markerIcon from 'leaflet/dist/images/marker-icon.png'
import markerShadow from 'leaflet/dist/images/marker-shadow.png'

delete L.Icon.Default.prototype._getIconUrl
L.Icon.Default.mergeOptions({
  iconRetinaUrl: markerIcon2x,
  iconUrl: markerIcon,
  shadowUrl: markerShadow,
})

const CAPE_FLATS_CENTER = [-34.0, 18.6]

const NEIGHBOURHOOD_CENTERS = {
  manenberg: [-33.9702, 18.5590],
  "mitchell's plain": [-34.0353, 18.6222],
  'mitchells plain': [-34.0353, 18.6222],
  khayelitsha: [-34.0356, 18.6597],
}

const fallbackZones = [
  { id: 1, name: 'NY108 Block', neighborhood: 'Manenberg', status: 'active', households: 62 },
  { id: 2, name: 'Silver City', neighborhood: 'Manenberg', status: 'pending', households: 48 },
  { id: 3, name: 'Tafelsig', neighborhood: "Mitchell's Plain", status: 'active', households: 180 },
  { id: 4, name: 'Rocklands', neighborhood: "Mitchell's Plain", status: 'active', households: 150 },
  { id: 5, name: 'Site C', neighborhood: 'Khayelitsha', status: 'active', households: 210 },
  { id: 6, name: 'Harare', neighborhood: 'Khayelitsha', status: 'pending', households: 95 },
]

const mapEl = ref(null)
const loadError = ref(false)
let mapInstance = null

function getCoords(neighborhood, index) {
  const key = (neighborhood || '').trim().toLowerCase()
  const base = NEIGHBOURHOOD_CENTERS[key] || CAPE_FLATS_CENTER
  const jitter = 0.006
  const angle = (index * 137.5 * Math.PI) / 180
  return [base[0] + Math.cos(angle) * jitter, base[1] + Math.sin(angle) * jitter]
}

function addZoneMarkers(zones) {
  zones.forEach((zone, index) => {
    const [lat, lng] = getCoords(zone.neighborhood, index)
    const marker = L.marker([lat, lng]).addTo(mapInstance)
    const statusLabel = zone.status === 'active' ? 'Active' : 'Pending'
    marker.bindPopup(
      `<strong>${zone.name}</strong><br/>${zone.neighborhood}<br/>Status: ${statusLabel}<br/>${zone.households} households`
    )
  })
}

async function loadZones() {
  try {
    const res = await fetch('/api/zones/map')
    if (!res.ok) throw new Error('bad response')
    const data = await res.json()
    addZoneMarkers(Array.isArray(data) && data.length ? data : fallbackZones)
  } catch (err) {
    loadError.value = true
    addZoneMarkers(fallbackZones)
  }
}

onMounted(() => {
  mapInstance = L.map(mapEl.value).setView(CAPE_FLATS_CENTER, 12)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors',
    maxZoom: 18,
  }).addTo(mapInstance)
  loadZones()
})

onBeforeUnmount(() => {
  if (mapInstance) mapInstance.remove()
})
</script>

<style scoped>
.zone-map-section { max-width: 1100px; margin: 0 auto; padding: 1rem; }
.zone-map { width: 100%; height: 420px; border-radius: 16px; overflow: hidden; box-shadow: 0 10px 30px rgba(0,0,0,.1); }
.map-note { margin: .75rem 0 0; color: #9aa8a4; font-size: .85rem; text-align: center; }
</style>
