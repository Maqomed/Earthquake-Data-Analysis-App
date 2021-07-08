<template>
  <div>
    <div id="mapContainer" class="basemap"></div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
export default {
  name: "BaseMap",

  data() {
    return {
      accessToken: process.env.VUE_APP_TOKEN,
    };
  },
  created() {},
  mounted() {
    mapboxgl.accessToken = this.accessToken;
    new mapboxgl.Map({
      container: "mapContainer",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [40.143105, 47.576927],
      zoom: 1,
    });
  },
};
</script>
<style scoped>
@media only screen and (max-width: 1200px) {
  .basemap {
    width: 100vw;
    height: 100vh;
    /* margin-top: 20px; */
  }
}

@media only screen and (min-width: 1200px) {
  .basemap {
    width: 900px;
    height: 100vh;
  }
}
.mapboxgl-popup {
  max-width: 200px;
}
</style>