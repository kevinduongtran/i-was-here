<template>
  <div class="map-container bg-dark w-100 h-100 rounded">
    <l-map
      class="rounded"
      ref="map"
      v-if="showMap"
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
      @ready="mapReady()"
      @click="mapClicked"
    >
      <l-tile-layer :url="url" :attribution="attribution" />


      <l-marker
        ref="cursor"
        name="cursor"
        :lat-lng="cursor.latlng"
        @click="markerClicked('cursor')"
      >
        <l-icon class-name="zone-label">
          <div v-show="cursor.visible">
            <div class="cursor-arrow"></div>
            <b-button class="cursor-button" variant="success">
              Add Point
            </b-button>
          </div>
        </l-icon>
      </l-marker>
      <l-marker :lat-lng="withPopup" name="userPoint" @click="markerClicked">
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import L from "leaflet";
import Constants from "../Constants.js";
import { latLng } from "leaflet";
import {
  LMap,
  LTileLayer,
  LMarker,
  LIcon,
  // LPopup,
  LTooltip,
} from "vue2-leaflet";

export default {
  name: "Map",
  props: {
    msg: String,
  },
  components: {
    LMap,
    LTileLayer,
    LIcon,
    // LMarker,
    // LPopup,
    // LTooltip,
  },
  data() {
    return {
      zoom: 4,
      center: latLng(Constants.MAP_LAT, Constants.MAP_LNG),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(47.41322, -1.219482),
      withTooltip: latLng(47.41422, -1.250482),
      currentCenter: latLng(47.41322, -1.219482),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.25,
        doubleClickZoom: false,
      },
      showMap: true,
      cursor: {
        latlng: latLng(0, 0),
        visible: false,
      },
    };
  },
  methods: {
    mapReady() {
      console.log("Map Ready");
      this.map = this.$refs.map;
    },
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    showLongText() {
      this.showParagraph = !this.showParagraph;
    },

    mapClicked(e) {
      console.log("map clicked");

      this.$refs.cursor.setLatLng(e.latlng);
      // console.log(this.$refs.cursor);
      this.cursor.visible = true;
    },
    markerClicked(e) {
      if (e == "cursor") {
        console.log("cursor clicked");
        this.cursor.visible = false;
        this.map.setCenter(this.cursor.latlng)
      } else {
        console.log("marker clicked");
      }
    },
    addPoint() {},
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.map-container {
  margin-left: 250px;
  max-width: calc(100% - 250px);
}
.cursor-button {
  width: 100px;
  transform: translate(-50px, -70px);
  z-index: 8;
}
.cursor-arrow {
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;

  border-top: 20px solid var(--success);
  transform: translate(-20px, -20px);
  z-index: -1;
}
</style>
