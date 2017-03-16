<template>
  <div id="app">
  	<navigation />
  	<div class="content">
	  	<div class="contentHotels">
	  		<sortbar 
	  			@namesort="nameSort"
	  			@citysort="citySort"
	  			@pricesort="priceSort"
	  			@ratingsort="ratingSort"
	  			@showmap="showMap"
	  			@showlist="showList"
	  		/>
			<hotels v-for="sample in samples" :key="sample.id" v-if="displayList">
				<a slot="link" :href="sample.deeplink" target="_blank">
					<img slot="image" :src="sample.image" :alt="sample.name" width="50%" />
					<div slot="content" class="hotel">
						<div class="hotel__title">{{sample.name}}</div>
						<div class="hotel__city">{{sample.city}}</div>
						<div class="hotel__price"><strong>€</strong>{{sample.price}}.00</div>
						<div class="hotel__category">
							<star-rating 
								:rating="sample.category" 
								:read-only="true" 
								:increment="0.5" 
								:star-size="20" 
								:show-rating="false"
								active-color="#0097d9"
							/>
						</div>
					</div>
				</a>
			</hotels>
	  	</div>
	  	<div class="contentMap" v-if="displayMap">
		  	<google-map :center="startPos" :zoom="10" class="mapContainer" @click="infoWinOpen=false">
			<gmap-info-window :options="infoOptions" :position="infoWindowPos" :opened="infoWinOpen" :content="infoContent" @closeclick="infoWinOpen=false" />
	  			  	<map-marker
	  			       v-for="m,i in startMarker"
	  			       :key="m.name"
	  			       :position="m.position"
	  			       :clickable="true"
	  			       :infoText="m.name"
	  			       @click="toggleInfoWindow(m,i)"
	  		       >
	  			   </map-marker>
	  			  </google-map>
	  	</div>
	  </div>
  </div>
</template>

<script>
	import Navigation from './components/Navigation.vue'
	import Hotels from './components/HotelContainer.vue'
	import Sortbar from './components/SortBar.vue'
	import axios from 'axios';
	import * as VueGoogleMaps from 'vue2-google-maps'
	import StarRating from 'vue-star-rating'
	export default {
	  name: 'app',
	  components: { Navigation, Hotels, StarRating, Sortbar, 'googleMap': VueGoogleMaps.Map,
	    'MapMarker': VueGoogleMaps.Marker, 'MapInfoWindow': VueGoogleMaps.MapInfoWindow  },
	  data() {
	  	return {
	  		samples: {},
	      	infoWinOpen: false,
	      	infoContent: '',
	      	currentMidx: null,
	      	infoWindowPos: {
		        lat: 0,
		        lng: 0
		    },
	        infoOptions: {
		        pixelOffset: {
		          width: 0,
		          height: -35
		        }
	        },
	        displayMap: true,
	        displayList: true,
	  	}
	  },
	  methods: {
	  	nameSort(){
	  		this.samples.sort(function(a, b) {
	  			const nameA=a.name.toLowerCase();
	  			const nameB=b.name.toLowerCase();
	  			if (nameA < nameB)
	  			        return -1 
	  			    if (nameA > nameB)
	  			        return 1
	  			    return 0
	  		})
	  	},
	  	citySort(){
	  		this.samples.sort(function(a, b) {
	  			const cityA=a.city.toLowerCase();
	  			const cityB=b.city.toLowerCase();
	  			if (cityA < cityB)
	  			        return -1 
	  			    if (cityA > cityB)
	  			        return 1
	  			    return 0
	  		})
	  	},
	  	priceSort() {
	  		this.samples.sort(function(a, b){
	    		return a.price-b.price
			})
	  	},
	  	ratingSort() {
	  		this.samples.sort(function(a, b){
	    		return b.category-a.category
			})
	  	},
	  	toggleInfoWindow(marker, idx) {
            this.infoWindowPos = marker.position;
            this.infoContent = marker.name;
            if (this.currentMidx == idx) {
              this.infoWinOpen = !this.infoWinOpen;
            }
            else {
              this.infoWinOpen = true;
              this.currentMidx = idx;
            }
	    },
	    showMap() {
	    	this.displayMap = true;
	    	this.displayList = false;
	    },
	    showList() {
	    	this.displayMap = false;
	    	this.displayList = true;
	    },
	  },
	  computed: {
	  	startMarker() {
	  		const testArr = [];
	  		for (var i = this.samples.length - 1; i >= 0; i--) {
	  			testArr.push({name: this.samples[i]['name'], position: { lat:this.samples[i]['location'].lat, lng: this.samples[i]['location'].lon}})
	  		}
	  		return testArr;
	  	},
	  	startPos() {
	  		const testStart = { lat:this.samples[0]['location'].lat, lng: this.samples[0]['location'].lon};
	  		return testStart;
	  	}
	  },
	  mounted() {
	  	axios.get('https://api.beach-inspector.com')
	  	  .then(response => {
	  	  	this.samples = response.data;
	  	  	})
	  	  .catch(error => console.log(error));
	  }
	}
</script>

<style>
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css');

	body 
	{
		margin: 0px;
		background : #f6f6f6;
	}
	a
	{
		text-decoration: none;
	    display: block;
	    width: 100%;
	    margin-top: 5px;
	}
	.content
	{
		width: 100%;
		height: 610px;
		overflow: hidden;
		display: block;
	}
	.contentHotels 
	{
		display: inline-block;
		width: 590px;
		height: 100%;
		overflow-y: auto;
		float: left;
		border-right: 2px solid #444444;
	}
	.contentMap
	{
		display: inline-block;
		float: left;
		height: 100%;
		overflow: hidden;
		width: calc(100% - 600px);
	}
	.mapContainer
	{
		height: 100%;
		width: 100%;
	}
	.hotel 
	{
		font-size: 15px;
	    color: #595959;
	    display: inline-block;
	    vertical-align: top;
	    padding: 20px;
	    width: 35%;
	}
	.hotel__title
	{
		font-weight: bold;
		font-size: 16px;
		color: #0097d9;
		margin-bottom: 10px;
	}
	.hotel__city
	{
		font-style: italic;
		margin-bottom: 10px;
	}
	.hotel__price
	{
		font-size: 14px;
		margin-bottom: 5px;
		margin-bottom: 10px;
	}
	@media only screen and (max-width: 767px) {
		.contentHotels 
		{
			display: block;
			width: 100%;
			height: 100%;
			float: left;
			border-right: none;
			overflow: auto;
		}
		.contentMap
		{
			display: block;
			float: left;
			height: 100%;
			position: fixed;
			top: 175px;
			width: 100%;
		}
		.hotel 
		{
			font-size: 15px;
		    color: #595959;
		    display: inline-block;
		    vertical-align: top;
		    padding: 5px 20px;
		    width: 35%;
		}
		.hotel__title
		{
			font-weight: bold;
			font-size: 16px;
			color: #0097d9;
			margin-bottom: 10px;
		}
		.hotel__city
		{
			font-style: italic;
			margin-bottom: 10px;
		}
		.hotel__price
		{
			font-size: 14px;
			margin-bottom: 5px;
			margin-bottom: 10px;
		}	
	}
</style>
