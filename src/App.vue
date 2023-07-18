<script>
import CardComponent from './components/CardComponent.vue';
import HeaderComponent from './components/HeaderComponent.vue';
import axios from "axios";
import { store } from './store.js';

export default {
	data() {
		return {
			store,
			
		}
	},
	components:{
		CardComponent,
		HeaderComponent
	},
	created(){
		axios
			.get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
			.then(response => {
				store.cards = response.data.data;
				store.currentPage = store.cards;
		});
		axios
			.get('https://db.ygoprodeck.com/api/v7/archetypes.php')
			.then(response => {
				store.archetypes = response.data;
		});
	},
}

</script>

<template>

	<HeaderComponent></HeaderComponent>

  	<main>
		<div class="container">

			<div class="cards-container">

				<h4>
					Found {{ store.currentPage.length }} cards
				</h4>
				
				<CardComponent v-for="(card, i) in store.currentPage" :key="i"
				:image-src="card.card_images[0].image_url"
				:title="card.name"
				:type="card.archetype"
				:class="card.archetype == null ? 'Unknown' : card.archetype">
				</CardComponent>

			</div>
		</div>
  	</main>

	<footer>
	</footer>

</template>

<style lang="scss" scoped>

@use './assets/scss/SingleStylingFiles/reset.scss' as *;
.container{
	max-width: 1200px;
	margin: 0 auto;
}

main{
	background-color: orange;
	padding-bottom: 30px;

	
	
	.cards-container{
		background-color: white;
		padding: 30px;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
	.cards-container > h4{
		background-color: #212529;
		color: white;
		padding: 20px;
		margin: 0;
		margin-bottom: 10px;
		width: 100%;
	}
}

</style>
