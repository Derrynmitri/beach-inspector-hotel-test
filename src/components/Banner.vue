<template>
	<div class="container">
		<navigation />
		<div class="banner">
			<div class="bannerTitle">
				Welcome
			</div>
			<div class="bannerDesc">
				to an awesome beach experience!
			</div>
		</div>
	</div>	
</template>

<script>
import Navigation from './Navigation.vue'
export default {
	components: { Navigation },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
	.container {
		width: 100%;
		height: 300px;
		background : #0097d9;
	}
	.banner {
		font-family: 'Roboto', sans-serif;
		margin:  70px auto;
		text-align: center;
	}
	.bannerTitle {
		font-size: 70px;
		color: #fff;
	}
	.bannerDesc {
		font-size: 20px;
		color: #fff;
	}
</style>