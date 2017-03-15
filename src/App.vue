<template>
  <div id="app">
  	<navigation />
  	<div class="content">
  		<sortbar 
  			@namesort="nameSort"
  			@citysort="citySort"
  			@pricesort="priceSort"
  			@ratingsort="ratingSort"
  		/>
		<hotels v-for="sample in samples" :key="sample.id">
			<img slot="image" :src="sample.image" :alt="sample.name">
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
		</hotels>
  	</div>
  </div>
</template>

<script>
import Navigation from './components/Navigation.vue'
import Hotels from './components/HotelContainer.vue'
import Sortbar from './components/SortBar.vue'
import axios from 'axios';
import StarRating from 'vue-star-rating'
import samples from './assets/hotels.json'
export default {
  name: 'app',
  components: { Navigation, Hotels, StarRating,Sortbar },
  data() {
  	return {
  		samples,
  		message: true
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
  	}
  },
  mounted() {
  	// axios.get('https://api.beach-inspector.com')
  	//   .then(function (response) {
  	//     console.log(response);
  	//   })
  	//   .catch(function (error) {
  	//     console.log(error);
  	//   });
  }
}
</script>

<style>
	body 
	{
		margin: 0px;
		background : #f6f6f6;
	}
	.content 
	{
		width: 600px;
		border-right: 2px solid #444444;
	}
	.hotel 
	{
		font-size: 15px;
		color: #595959;
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
</style>
