<template>
	<div class="hotelContainer">
		<slot name="link">
			<div class="imageContainer">
				<slot name="image"></slot>
			</div>
			<div class="contentContainer">
				<slot name="content"></slot>
			</div>
		</slot>
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
		height: 210px;
		display: block;
		margin-left: 10px;
		border-bottom: 2px solid #d9d9d9;
	}
	.imageContainer 
	{
		display: inline-block;
		float: left;
		width: auto;
		height: 200px;
		padding-top: 5px;
	}
	.contentContainer
	{
		display: inline-block;
		padding: 20px;
		width: auto;
		height: auto;
	}
</style>