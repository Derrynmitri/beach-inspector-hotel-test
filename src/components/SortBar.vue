<template>
	<div class="sortContainer">
		<h4 class="title">Sort By</h4>
		<div class="button-group">
			<button class="selected">Name <i class="fa fa-arrow-down" aria-hidden="true" style="font-size:10px;"/></button>
			<button class="button">City <i class="fa fa-arrow-down" aria-hidden="true" style="font-size:10px;" /></button>
			<button class="button">Price <i class="fa fa-arrow-down" aria-hidden="true" style="font-size:10px;" /></button>
			<button class="button">Rating <i class="fa fa-arrow-down" aria-hidden="true" style="font-size:10px;" /></button>
		</div>
	</div>	
</template>

<script>
	export default {
		
	}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css');
	.sortContainer 
	{
		font-family: 'Roboto', sans-serif;
		background : #444444;
		color: #fff;
		width: 100%;
		height: 50px;
		display: block;
		margin: auto;
		top: 0;
		left: 0;
	}
	.title
	{
		display: inline-block;
		margin: 15px 15px;
	}
	.button-group
	{
		display: inline-block;
		float: right;
		margin: 12px 15px;
	}
	button
	{
		border: none;
		color: #fff;
		background-color: #444444;
		height: 25px;
		width: 75px;
		border-radius: 2px;
	}
	button.selected
	{
		background-color: #0097d9;
	}
	button:hover 
	{
		cursor: pointer;
	}
</style>