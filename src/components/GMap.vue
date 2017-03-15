<template>
	<div style="width: 600px; height: 100%">
	  <google-map :center="center" :zoom="10" @rightclick="newMarker" class="mapContainer">
	  	<map-marker
	       v-for="m in markers"
	       :position="m.position"
	       :opacity="m.opacity"
	       :draggable="m.draggable"
	       @position_changed="updMarker(m, $event)"
		   @dragend="logMarker(m)"
		   :key="m.id"
       >
	   </map-marker>
	  </google-map>
	</div>
</template>

<script>
import * as VueGoogleMaps from 'vue2-google-maps'

export default {
  data () {
    return {
      markers: [{ position: {lat:-19.72146407652849, lng: -44.197998046875} }, { position: {lat:-19.867476750284457, lng: -44.14581298828125}}],
      center: { lat: -19.8934596, lng: -44.0586543 }
    }
  },
  methods: {
    newMarker (mouseArgs) {
      const createdMarker = this.addMarker()
      createdMarker.position.lat = mouseArgs.latLng.lat()
      createdMarker.position.lng = mouseArgs.latLng.lng()
      console.log(createdMarker.position.lat, createdMarker.position.lng)
    },
    addMarker () {
      this.markers.push({
        position: { lat: 48.8538302, lng: 2.2982161 },
        draggable: true,
        enabled: true
      })
      return this.markers[this.markers.length - 1]
    },
    updMarker (m, event) {
      m.position = {
        lat: event.lat(),
        lng: event.lng()
      }
    },
    logMarker (m) {
      console.log(m.position.lat, m.position.lng)
    }
  },
  components: {
    'googleMap': VueGoogleMaps.Map,
    'MapMarker': VueGoogleMaps.Marker
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
	.mapContainer {
		font-family: 'Roboto', sans-serif;
		background : #ffffff;
		width: 600px;
		height: 600px;
		display: block;
		margin: auto;
		top: 0;
		left: 0;
	}
</style>