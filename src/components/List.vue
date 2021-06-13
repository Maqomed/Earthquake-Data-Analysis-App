<template>
  <div class="list">
    <div class="buttons btn-group">
      <div class="dropdown d-flex flex-column">
        <button
          v-on:click="dropdownStatus = !dropdownStatus"
          type="button"
          class="filterBtn btn btn-primary dropdown-toggle"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false"
        >
          Filter
        </button>
        <div v-if="dropdownStatus">
          <ul class="list-group">
            <li v-on:click="showAll()" class="list-group-item">Show All</li>
            <li v-on:click="showHighMagnitudes()" class="list-group-item">
              Magnitude 2.5+
            </li>
            <li v-on:click="showLowMagnitudes()" class="list-group-item">
              Magnitude 2.5-
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="card cardHigh">
      <div class="card-header">Highest magnitude earthquake</div>
      <div class="card-body">
        <h5 class="card-title">
          {{ this.features[this.indexOfMaxMag].properties.title }}
        </h5>
        <p class="card-text">
          Detected magnitude:
          {{ this.features[this.indexOfMaxMag].properties.mag }}
        </p>
        <p class="card-text">
          Detected time:
          {{ new Date(this.features[this.indexOfMaxMag].properties.time) }}
        </p>
        <a v-on:click="showOnMapMaxMagnitudeZone()" class="btn btn-primary"
          >Show on map</a
        >
      </div>
    </div>
    <div class="card cardLow">
      <div class="card-header">Lowest magnitude earthquake</div>
      <div class="card-body">
        <h5 class="card-title">
          {{ this.features[this.indexOfMinMag].properties.title }}
        </h5>
        <p class="card-text">
          Detected magnitude:
          {{ this.features[this.indexOfMinMag].properties.mag }}
        </p>
        <p class="card-text">
          Detected time:
          {{ new Date(this.features[this.indexOfMinMag].properties.time) }}
        </p>
        <a v-on:click="showOnMapMinMagnitudeZone()" class="btn btn-primary"
          >Show on map</a
        >
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";

export default {
  props: {
    earthquakeData: Array,
  },
  data() {
    return {
      dropdownStatus: false,
      features: [],
      indexOfMaxMag: "",
      indexOfMinMag: "",
      highMagnitudeIndexes: [],
      lowMagnitudeIndexes: [],
      colorOfHighMag: "#c84b31",
      colorOfLowMag: "#346751",
      mainMarkerColor: "#125d98",
      markers: [],
      map: new mapboxgl.Map({
        container: "mapContainer",
        style: "mapbox://styles/mapbox/streets-v11",
        center: [40.143105, 47.576927],
        zoom: 1,
      }),
      max: -1,
      min: 10,
    };
  },
  created() {
    this.features = this.earthquakeData;
    for (var i = 0; i < this.features.length; i++) {
      if (this.features[i].properties.mag >= this.max) {
        this.max = this.features[i].properties.mag;
        this.indexOfMaxMag = i;
      }
    }
    for (var j = 0; j < this.features.length; j++) {
      if (this.features[j].properties.mag <= this.min) {
       this.min = this.features[j].properties.mag;
        this.indexOfMinMag = j;
      }
    }
  },
  mounted() {
    this.showAll();
    this.map.addControl(new mapboxgl.NavigationControl());
    this.max = this.features[0].properties.mag;
    this.min = this.features[0].properties.mag;
    
  },
  methods: {
    showAll() {
      this.clearOldMarkers();
      for (let i = 0; i < this.features.length; i++) {
        this.showPopup(i, this.mainMarkerColor);
      }
      this.alignMapViewCenter();
    },
    showOnMapMaxMagnitudeZone() {
      this.clearOldMarkers();
      this.showPopup(this.indexOfMaxMag, this.colorOfHighMag);
      this.addNewMarkers();
      this.map.flyTo({
        center: [
          this.features[this.indexOfMaxMag].geometry.coordinates[0],
          this.features[this.indexOfMaxMag].geometry.coordinates[1],
        ],
        essential: true, // this animation is considered essential with respect to prefers-reduced-motion
      });
    },
    showOnMapMinMagnitudeZone() {
      this.clearOldMarkers();
      this.showPopup(this.indexOfMinMag, this.colorOfLowMag);
      this.addNewMarkers();
      this.map.flyTo({
        center: [
          this.features[this.indexOfMinMag].geometry.coordinates[0],
          this.features[this.indexOfMinMag].geometry.coordinates[1],
        ],
        essential: true, // this animation is considered essential with respect to prefers-reduced-motion
      });
    },
    showPopup(index, magColor) {
      var convertedTime = new Date(this.features[index].properties.time);
      var popup = new mapboxgl.Popup().setHTML(
        "<b>Region: </b>" +
          this.features[index].properties.title +
          "<br>" +
          "<b>Time: </b>" +
          convertedTime +
          "<br>" +
          "<b>Depth: </b>" +
          this.features[index].geometry.coordinates[2] +
          "km" +
          "<br>" +
          "<b>Magnitude: </b>" +
          this.features[index].properties.mag
      );
      var marker = new mapboxgl.Marker({ color: magColor })
        .setLngLat([
          this.features[index].geometry.coordinates[0],
          this.features[index].geometry.coordinates[1],
        ])
        .setPopup(popup);
      this.markers.push(marker);
      if (magColor === this.mainMarkerColor) {
        this.markers[index]
          .setPopup(popup) // sets a popup on this marker
          .addTo(this.map);
      }
    },
    showHighMagnitudes() {
      var numberOfHighMagnitudes = 0;
      this.clearOldMarkers();
      for (var i = 0; i < this.features.length; i++) {
        if (this.features[i].properties.mag > 2.5) {
          this.highMagnitudeIndexes.push(i);
          numberOfHighMagnitudes++;
        }
      }
      for (var j = 0; j < numberOfHighMagnitudes; j++) {
        this.showPopup(this.highMagnitudeIndexes[j], this.colorOfHighMag);
      }
      this.addNewMarkers();
      this.alignMapViewCenter();
    },
    showLowMagnitudes() {
      var numberOfLowMagnitudes = 0;
      this.clearOldMarkers();
      for (var k = 0; k < this.features.length; k++) {
        if (this.features[k].properties.mag < 2.5) {
          console.log(this.features[k].properties.mag)
          this.lowMagnitudeIndexes.push(k);
          numberOfLowMagnitudes++;
        }
      }
      for (var f = 0; f < numberOfLowMagnitudes; f++) {
        this.showPopup(this.lowMagnitudeIndexes[f], this.colorOfLowMag);
      }
      this.addNewMarkers();
      this.alignMapViewCenter();
    },
    clearOldMarkers() {
      for (var i = 0; i < this.markers.length; i++) {
        this.markers[i].addTo(this.map).remove();
      }
      this.markers = [];
    },
    addNewMarkers() {
      for (var i = 0; i < this.markers.length; i++) {
        this.markers[i].addTo(this.map);
      }
    },
    alignMapViewCenter() {
      this.map.flyTo({
        center: [40.143105, 47.576927],
        essential: true, // this animation is considered essential with respect to prefers-reduced-motion
      });
    },
  },
};
</script>
<style scoped>
@media only screen and (min-width: 1200px) {
  .filterBtn {
    width: 360px;
  }
  .list {
    margin-top: 10px;
    margin-right: 10px;
    margin-left: 10px;
    height: 100vh;

    width: 500px;
    font-family: "Merriweather Sans", sans-serif;
  }

  .cardHigh {
    margin-top: 50px;
  }
  .cardLow {
    margin-top: 10px;
  }
  .buttons {
    position: absolute;
    z-index: 10;
  }

  li {
    cursor: pointer;
    background-color: #dddddd;
  }
  li:hover {
    background-color: gray;
  }
}

@media only screen and (max-width: 1200px) {
  .filterBtn {
    width: 100px;
    margin-top: 2px;
  }
  .card {
    display: none;
  }
  .list {
    position: absolute;
  }
  .buttons {
    position: absolute;
    z-index: 10;
  }
  li {
    cursor: pointer;
  }
  li:hover {
    background-color: gray;
  }
}
</style>