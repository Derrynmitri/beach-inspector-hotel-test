<!-- A component used to house all the sorting buttons as well as the mobile display options -->
<template>
	<div class="sortContainer">
		<h4 class="title">Sort By</h4>
		<div class="button-group">
			<button  
				v-for="button in buttons" 
				:class="{ 'selected' : button.selected }" 
				@click="$emit(button.func), select(button)"
			>
				{{button.title}} <i class="fa fa-arrow-down" aria-hidden="true" style="font-size:10px;"/>
			</button>
		</div>
		<div class="mobileDisplay">
			<button type="button" class="button" @click="$emit('showlist')">List</button>
			<button type="button" class="button" @click="$emit('showmap')">Map</button>
		</div>	
	</div>
</template>

<script>
	export default {
		data() {
			return {
				buttons: [
					{ title:'Name', func: 'namesort', selected: false },
					{ title:'City', func: 'citysort', selected: false },  
					{ title:'Price', func: 'pricesort', selected: false }, 
					{ title:'Category', func: 'ratingsort', selected: false } 
				],
			}
		},
		methods: {
			select(buttonSelected){
				this.buttons.forEach(button => {
					button.selected = false;
				});
				this.buttons.forEach(butt => {
					buttonSelected.selected = (buttonSelected.name == butt.name);
				});
			}
		},
	}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
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
		margin: 12px 5px;
	}
	button
	{
		border: none;
		color: #fff;
		background-color: #444444;
		height: 25px;
		width: 80px;
		border-radius: 2px;
	}
	button.selected
	{
		background-color: #0097d9;
	}
	button:hover 
	{
		cursor: pointer;
		background-color: #0097d9;
	}
	.mobileDisplay 
	{
		display: none;
	}
	@media only screen and (max-width: 767px) {
		.sortContainer 
		{
			height: auto;
			display: block;
			margin: -15px 0px 0 0;
			padding: 10px 0px 0px 0px;
		}
		.title
		{
			display: block;
			margin: 15px 0px 0px 10px;
		}
		.button-group
		{
			display: block;
			height: auto;
			overflow: hidden;
			float: left;
			margin: 12px 5px;
		}
		.mobileDisplay 
		{
			display: block;
			padding: 10px;
			background: #0097d9;
			clear: both;
			text-align: center;
		}
		button
		{
			width: 75px;
			padding: 0px;
		}
	}
</style>