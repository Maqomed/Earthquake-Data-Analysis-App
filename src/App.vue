<template>
  <div id="app">
    <Header msg="Welcome to Your Vue.js App" />
    <div class="bodyComponents bd-highlight mb-3">
      <List
        v-if="this.earthquakeData.length > 0"
        :earthquakeData="this.earthquakeData"
      ></List>
      <Map></Map>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import List from "./components/List.vue";
import Map from "./components/Map.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    Header,
    List,
    Map,
  },
  data() {
    return {
      earthquakeData: [],
    };
  },
  mounted() {
    axios
      .get(
        "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson"
      )
      .then((response) => {
        this.earthquakeData = response.data.features;
      });
  },
};
</script>

<style>
@media only screen and (max-width: 1200px) {
  .bodyComponents {
    display: flex;
    flex-direction: column;
  }
}
@media only screen and (min-width: 1200px) {
  .bodyComponents {
    display: flex;
    flex-direction: row;
  }
}
</style>
