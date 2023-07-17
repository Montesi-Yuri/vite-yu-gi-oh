<script>
import CardComponent from './components/CardComponent.vue'
import axios from "axios";
import { store } from './store.js';

export default {
	data() {
		return {
			store,
			archetypes:[

			],
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
	created(){
		axios
			.get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
			.then(response => {
				store.cards = response.data.data;

				for (let i = 0; i < this.store.cards.length; i++) {
					const card = this.store.cards[i];
					console.log(card.archetype);
					if(!this.archetypes.includes(card.archetype) && card.archetype != null){
						this.archetypes.push(card.archetype);
						console.log(this.archetypes, 'classi carte')
					}
					else if(!this.archetypes.includes('Unknown') && card.archetype == null){
						this.archetypes.push('Unknown');
					}
				}	
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

			<select name="" id="">

				<option value="" v-for="(type,i) in this.archetypes" :key="i">
					{{ type }}
				</option>

			</select>

			<button>
				Search
			</button>

			<div class="cards-container">
				<h4>
					Found {{ store.cards.length }} cards
				</h4>
				<CardComponent v-for="(card, i) in store.cards" :key="i"
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
	select{
		margin: 20px;
		padding: 5px;
		border: 1px solid black;
		border-radius: 5px;
	}
	button{
		padding: 5px;
		border: 1px solid black;
		border-radius: 5px;
		background-color: #212529;
		color: white;
		cursor: pointer;
		&:hover{
			background-color: grey;
		}
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
