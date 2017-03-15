<template>
  <div id="app">
  	<navigation />
  	<div class="content">
  		Content
  	</div>
  </div>
</template>

<script>
import Navigation from './components/Navigation.vue'

export default {
  name: 'app',
  components: { Navigation },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style>
	body {
		margin: 0px;
	}
	.content {
		margin: 20px 100px;
	}
</style>
