<template>
	<div class="headerContainer">
		<h3 class="logo">Beach-Inspector</h3>
	</div>	
</template>

<script>
	export default {
		
	}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
	.headerContainer 
	{
		font-family: 'Roboto', sans-serif;
		background : #0097d9;
		color: #fff;
		width: 100%;
		height: 50px;
		display: block;
		margin: auto;
		top: 0;
		left: 0;
	}
	.logo 
	{
		display: inline-block;
		margin: 15px 0 0 15px;
	}
	@media only screen and (max-width: 767px) {
		.headerContainer 
		{
			height: 50px;
		}
	}
</style>