<template>
	<div class="hotelContainer">
		<div class="imageContainer">
			<slot name="image"></slot>
		</div>
		<div class="contentContainer">
			<slot name="content"></slot>
		</div>
	</div>	
</template>

<script>
	export default {
		
	}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
	.hotelContainer 
	{
		font-family: 'Roboto', sans-serif;
		background : #f6f6f6;
		color: #000;
		width: 550px;
		height: 200px;
		display: block;
		margin-bottom: 10px;
	}
	.imageContainer 
	{
		display: inline-block;
		float: left;
		width: auto;
		height: 200px;
	}
	.contentContainer
	{
		display: inline-block;
		padding: 20px;
		width: auto;
		height: auto;
	}
</style>