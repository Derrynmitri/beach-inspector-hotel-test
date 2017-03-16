<template>
  <div id="app">
  	<navigation />
	<div v-if="!samples"><center>An error occured please try again later</center></div>
  	<div class="content" v-if="samples">
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
							<p class="hotel__title">{{sample.name}}</p>
							<p class="hotel__city">{{sample.city}}</p>
							<p class="hotel__price"><strong>€</strong>{{sample.price}}.00</p>
							<div class="hotel__category">
								<star-rating 
									:rating="parseFloat(sample.category)" 
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
					<gmap-info-window 
						:options="infoOptions" 
						:position="infoWindowPos" 
						:opened="infoWinOpen" 
						:content="infoContent" 
						@closeclick="infoWinOpen=false" 
					/>
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
	  components: { 
	  	Navigation, 
	  	Hotels, 
	  	StarRating, 
	  	Sortbar, 
	  	'googleMap': VueGoogleMaps.Map,
	    'MapMarker': VueGoogleMaps.Marker, 
	    'MapInfoWindow': VueGoogleMaps.MapInfoWindow  
	  },
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
	  		});
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
	  		});
	  	},
	  	priceSort() {
	  		this.samples.sort(function(a, b){
	    		return a.price-b.price
			});
	  	},
	  	ratingSort() {
	  		this.samples.sort(function(a, b){
	    		return b.category-a.category
			});
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
	  			testArr.push({name: this.samples[i]['name'], position: { lat:this.samples[i]['location'].lat, lng: this.samples[i]['location'].lon }})
	  		}
	  		return testArr;
	  	},
	  	startPos() {
	  		let testStart = {};
	  		if (typeof this.samples[0] == 'undefined') {
	  			testStart = {lat: -26.2041, lng: 28.0473};
	  		} else {
	  			testStart ={ lat:this.samples[0]['location'].lat, lng: this.samples[0]['location'].lon };
	  		}
	  		return testStart;
	  	}
	  },
	  mounted() {
	  	axios.get('https://api.beach-inspector.com')
	  	  .then(response => {
	  	  	this.samples = response.data;
	  	  	})
	  	  .catch(error => {
	  	  	console.log(error);
	  	  	this.samples = false;
	  	  });
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
		height: calc(100vh - 50px);
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
	    width: 40%;
	}
	.hotel__title
	{
		font-weight: bold;
		font-size: 16px;
		color: #0097d9;
		margin: 0 0 10px 0;
	}
	.hotel__city
	{
		font-style: italic;
		margin: 0 0 10px 0;
	}
	.hotel__price
	{
		font-size: 14px;
		margin: 0 0 10px 0;
	}
	@media only screen and (max-width: 767px) {
		.contentHotels 
		{
			width: 100%;
			height: 100%;
			float: left;
			border-right: none;
			overflow: auto; 
		}
		.contentMap
		{
			height: 100%;
			width: 100%;
			margin: 125px 0px 0px -100%;
		}
		.hotel 
		{
		    width: 35%;
		}
		.hotel__title
		{
			margin: 0;
		}
		.hotel__city
		{
			margin: 0;
		}
		.hotel__price
		{
			margin: 0;
		}
	}
</style>
