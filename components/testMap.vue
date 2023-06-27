<template>
  <l-map
    style="width: 100%; height: 100%"
    ref="map"
    @ready="hookUpDraw"
    :center="mapLayer.center"
    :zoom="mapLayer.zoom"
  >
    <template v-if="mapReady">
      <l-tile-layer
        :url="mapLayer.url"
        :attribution="mapLayer.attribution"
      ></l-tile-layer>
    </template>
  </l-map>
</template>

<script setup>
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer } from "@vue-leaflet/vue-leaflet";
import "@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css";
// import "@geoman-io/leaflet-geoman-free";

const map = ref(null);
const mapObject = ref(null);
const mapReady = ref(false);

const mapLayer = {
  zoom: 12,
  center: [51.505, -0.09],
  url: "http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  attribution:
    '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
};

const geofence = ref([]);

const hookUpDraw = async () => {
  await import("@geoman-io/leaflet-geoman-free");
  console.log("MAP", map);
  mapObject.value = map.value.leafletObject;
  mapReady.value = true;
  console.log("mapObject before", mapObject.value);
  
  console.log("mapObject after", mapObject.value);
  mapObject.value.pm.setLang("en_gb");
  mapObject.value.pm.addControls({
    position: "topleft",
    drawCircle: true,
  });
  mapObject.value.on("pm:drawstart", ({ workingLayer }) => {
    workingLayer.on("pm:vertexadded", (e) => {
      console.log("vertexadded", e);
      geofence.value.push(e);
    });
  });

  mapObject.value.on("pm:drawend", () => {
    console.log("drawend", geofence.value);
  });
};

//onBeforeMount(async () => {
//  console.log('test')
//  window.L = await import("leaflet/dist/leaflet-src.esm");
//  await import("@geoman-io/leaflet-geoman-free");
//  await import("@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css");
//});
</script>
