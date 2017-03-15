<template>
  <div id="app">
  	<navigation />
  	<div class="content">
  		<ul>
  			<hotels v-for="sample in samples">
  				<img slot="image" :src="sample.image" :alt="sample.name">
  				<div slot="content">
  					<div>{{sample.name}}</div>
  					<div>{{sample.city}}</div>
  					<div>{{sample.price}}</div>
  					<div>{{sample.category}}</div>
  				</div>
  			</hotels>
  		</ul>
  	</div>
  </div>
</template>

<script>
import Navigation from './components/Navigation.vue'
import Hotels from './components/HotelContainer.vue'
import axios from 'axios';
import samples from './assets/hotels.json'
export default {
  name: 'app',
  components: { Navigation, Hotels },
  data() {
  	return {
  		samples
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
	body {
		margin: 0px;
	}
	.content {
		margin: 20px 0px;
	}
</style>
