<template>
  <div v-bind:style="{height:window.height+'px' , width:'100%' }">
    <l-map
      v-if="showMap"
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      style="height: 100%"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
    >
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-marker :lat-lng="[38.9637451, 35.2433205]" :icon="iconRed" />
      <l-marker :lat-lng="[36.9637451, 35.2433205]" :icon="iconRed" />
      <l-marker :lat-lng="[39.9637451, 32.233205]" :icon="iconRed" />
      <l-marker :lat-lng="[40.9637451, 34.2233205]" :icon="iconRed" />
      <l-marker :lat-lng="[33.9637451, 35.2453205]" :icon="iconRed" />
      <l-marker :lat-lng="[39.9637451, 35.2453205]" :icon="iconRed" />
      <l-marker :lat-lng="[41.0637451, 29.2453205]" :icon="iconRed" />
      <l-marker :lat-lng="mapMe" :icon="iconMe" />
      <l-marker :lat-lng="withPopup">
        <l-popup>
          <div @click="innerClick">
            <p v-show="showParagraph">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
              sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
              Donec finibus semper metus id malesuada.
            </p>
          </div>
        </l-popup>
      </l-marker>
      <l-marker :lat-lng="withTooltip"></l-marker>
      <l-control-scale position="topright" :imperial="false" :metric="true"></l-control-scale>
      <l-control-zoom position="bottomright"></l-control-zoom>
    </l-map>
  </div>
</template>
<script>
import { latLng } from "leaflet";
import "@ansur/leaflet-pulse-icon/dist/L.Icon.Pulse.js";

import L from "leaflet";
import {
  LMap,
  LTileLayer,
  LMarker,
  LPopup,
  LControlZoom,
  LControlScale
} from "vue2-leaflet";

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LControlZoom,
    LControlScale
  },
  data() {
    return {
      iconMe: L.icon.pulse({ iconSize: [20, 20], color: "#00ff32",fillColor:"#00ff32" }),
      iconRed: L.icon.pulse({ iconSize: [20, 20], color: "red" }),
      zoom: 7,
      center: latLng(38.9637451, 35.2433205),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(38.9637451, 35.2433205),
      mapMe: latLng(38.9637451, 35.2433205),
      withTooltip: latLng(38.9637451, 35.2433205),
      currentZoom: 11.5,
      currentCenter: latLng(38.9637451, 35.2433205),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5,
        zoomControl: false
      },
      window: {
        width: 0,
        height: 0
      },
      showMap: true,
      lat: "",
      lon: ""
    };
  },
  created() {
    this.myFunction();

    //   this.myFunction();
    //           this.$getLocation(options)
    //   .then(coordinates => {
    //     console.log(coordinates);
    //   });
    // this.icon= L.icon.pulse({iconSize:[20,20],color:'red'});
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    myFunction: function() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition);
      } else {
        this.error = "Geolocation is not supported.";
      }
    },
    showPosition: function(position) {
      this.mapMe=latLng(position.coords.latitude,position.coords.longitude);
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
    innerClick() {
      alert("Click!");
    },
    handleResize() {
      this.window.width = window.innerWidth;
      this.window.height = window.innerHeight;
    }
  }
};
</script>

<style>
.leaflet-pulsing-icon {
  border-radius: 100%;
  box-shadow: 1px 1px 8px 0 rgba(0, 0, 0, 0.75);
}

.leaflet-pulsing-icon:after {
  content: "";
  border-radius: 100%;
  height: 300%;
  width: 300%;
  position: absolute;
  margin: -100% 0 0 -100%;
}

@keyframes pulsate {
  0% {
    transform: scale(0.1, 0.1);
    opacity: 0;
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    filter: alpha(opacity=0);
  }
  50% {
    opacity: 1;
    -ms-filter: none;
    filter: none;
  }
  100% {
    transform: scale(1.2, 1.2);
    opacity: 0;
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    filter: alpha(opacity=0);
  }
}
</style>
