<script>
import CardComponent from './components/CardComponent.vue'
import axios from "axios";
import { store } from './store.js';

export default {
  data() {
	return {
		store,
	}
  },
  components:{
	CardComponent
  },
  methods:{
	getImgPath: function(src) {
          return new URL(`${src}`, import.meta.url).href
        },
  },	
  created() {
	axios
            .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
            .then(response => {
                console.log(response.data);
                store.cards = response.data.data;
				console.log(this.store);
            });
	},
}

</script>

<template>

	<header>
		<div>
			<img :src="getImgPath('./assets/img/yugioh-logo.png')" alt="Yu-Gi-Oh logo">
			<h1>
				Yu-Gi-Oh Api
			</h1>
		</div>
	</header>

  <main>
		<div class="container">
			<input type="text">

			<div class="cards-container">
				<h4>
					Found {{ store.cards.length }} cards
				</h4>
				<CardComponent v-for="(card, i) in store.cards" :key="i"
				:image-src="card.card_images[0].image_url"
				:title="card.name"
				:type="card.archetype">
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

header{


	> div{
		padding-left: 20px;
	}
	img{
		width: 100px;
		vertical-align: middle;
	}
	h1{
		display: inline-block;
		vertical-align: middle;
	}
}

main{
	background-color: orange;
	padding-bottom: 30px;

	input{
		margin: 20px;
	}

	.cards-container{
		background-color: white;
		padding: 30px;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
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
