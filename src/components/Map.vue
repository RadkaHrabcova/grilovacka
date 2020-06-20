
<template>
  <div style="height:100%; width:100%;">
    <l-map :zoom="zoom" :center="center" :options="mapOptions">
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-marker
        v-for="(item, index) in grills"
        :key="index"
        :lat-lng="item.position"
        @click="grillIndex(item.id)"
        :icon="oneGrill && oneGrill.id === item.id ? icon : undefined"
      >
        <l-tooltip>{{item.name}}</l-tooltip>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { latLng, icon } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup, LTooltip } from "vue2-leaflet";

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LTooltip
  },
  props: {
    grills: {
      type: Array,
      required: true
    },
    oneGrill: { type: Object }
  },
  data() {
    return {
      icon: icon({
        iconUrl: require("../assets/icons/red-pin.png"),
        iconSize: [25, 41],
        iconAnchor: [12, 41]
      }),
      zoom: 13,
      center: latLng(49.2079947, 16.6066672),
      url: "https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(49.2079947, 16.6066672),
      currentZoom: 11.5,
      currentCenter: latLng(49.2079947, 16.6066672),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5
      },
      showMap: true
    };
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    showLongText() {
      this.showParagraph = !this.showParagraph;
    },
    innerClick() {
      alert("Click!");
    },
    grillIndex(index) {
      this.$emit("indexDetail", index);
    },
    setcenter(position) {
      this.center = position;
    }
  }
};
</script>

<style>
</style>