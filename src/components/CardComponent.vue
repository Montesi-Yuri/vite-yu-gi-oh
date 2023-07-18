<script>

export default {
  data() {
    return {
    }
  },
  props:{
	imageSrc: {
		String,
		default: 'Unknown'
	},
	title: {
		String,
		default: 'Unknown'
	},
	type: {
		String,
		default: 'Unknown'
	}
  }
}

</script>

<template>
	<div class="single-card">
		<div>
			<img :src="imageSrc" :alt="title">
		</div>
		<h3>
			{{ title }}
		</h3>
		<h4>
			{{ type }}
		</h4>
	</div>
</template>

<style lang="scss" scoped>

.single-card{
		background-color: orange;
		width: 200px;
		height: 420px;
		padding: 5px;
		border: 1px solid black;
		text-align: center;
		margin: 10px 10px;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		
		img{
			width: 100%;
		}
		h3{
			width: 100%;
			text-transform: uppercase;
			color: white;
			margin-bottom: 0;
		}
		h4{
			width: 100%;
			margin-bottom: 0;
		}
	}

</style>
