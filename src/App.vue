<script>
import Component1 from './components/Component1.vue'
import axios from "axios";
import { store } from './store.js';

export default {
  data() {
	return {
		store,
		
	}
  },
  components:{
	Component1
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
					Found N cards
				</h4>

				<div class="single-card">

					<div>
						<img :src="store.cards[0].card_images[0].image_url" alt="Card Image">
					</div>

					<h3>
						{{ store.cards[0].name}}
					</h3>

					<h4>
						{{ store.cards[0].archetype}}
					</h4>

				</div>
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
	height: calc(100vh - 100px);

	input{
		margin: 20px;
	}

	.cards-container{
		background-color: white;
		padding: 20px;
	}
	.cards-container > h4{
		background-color: #212529;
		color: white;
		padding: 20px;
		margin: 0;
	}
	.single-card{
		background-color: orange;
		width: 200px;
		padding: 5px;
		border: 1px solid black;
		text-align: center;

		img{
			width: 100%;
		}
		h3{
			text-transform: uppercase;
			color: white;
		}
		
	}
}

</style>
