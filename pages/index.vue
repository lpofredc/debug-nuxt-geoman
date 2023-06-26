<template>
  <div style="height: 100vh; width: 100%">
    <l-map
      ref="map"
      :useGlobalLeaflet="false"
      :zoom="zoom"
      :center="[47.41322, -1.219482]"
      @ready="hookUpDraw"
    >
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        layer-type="base"
        name="OpenStreetMap"
      ></l-tile-layer>
    </l-map>
  </div>
</template>

<script setup>
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer } from "@vue-leaflet/vue-leaflet";
import "@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css";

const zoom = ref(2);
const mapObject = ref();
const map = ref();
const hookUpDraw = async () => {
  if (process.client) {
    if (!map.pm) {
      await import(
        /* webpackChunkName: "leaflet-geoman" */ "@geoman-io/leaflet-geoman-free"
      );
      mapObject.value = map.value.leafletObject;
      mapObject.value.PM.reInitLayer(map);
    }
  }
};
</script>

<style></style>
