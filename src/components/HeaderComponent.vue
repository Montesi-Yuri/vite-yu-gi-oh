<script>
import ArchetypesCounter from './ArchetypesCounter.vue';
import { store } from '../store.js';
import axios from "axios";

export default {
  data() {
    return {
        store
    }
  },
  components:{
    ArchetypesCounter
  },
  methods:{
		getImgPath: function(src) {
			return new URL(`${src}`, import.meta.url).href
		},
		getSelectItems(){
			store.currentPage = [];
			axios
			.get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=250&offset=0')
			.then(response => {
				
				for (let i = 0; i < response.data.data.length; i++) {
					const card = response.data.data[i];
					if (card.archetype == store.classSelect ) {
						store.currentPage.push(card);
					}
				}
		});
		}
	},	
}

</script>

<template>

    <section class="header-top">
        <div>
			<img :src="getImgPath('../assets/img/yugioh-logo.png')" alt="Yu-Gi-Oh logo">
			<h1>
				Yu-Gi-Oh Api
			</h1>
		</div>
    </section>

    <section class="header-bottom">
        <div class="cards-filter">
            <label for="type-select">Search by type</label>
            <select name="type-select" v-model="store.classSelect" >
                <option :value="type.archetype_name" v-for="(type,i) in store.archetypes" :key="i">
                    {{ type.archetype_name }}
                </option>
            </select>
            <button @click.prevent="getSelectItems()" @click="$emit('typeTotal')">
                Search
            </button>
            <ArchetypesCounter></ArchetypesCounter>
        </div>
    </section>

</template>

<style lang="scss" scoped>

.container{
	max-width: 1200px;
	margin: 0 auto;
}
.header-top{
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
.header-bottom{
    .cards-filter{
		padding: 20px;
        background-color: orange;
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: center;

		select{
            margin: 0 10px;
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
	}
}

</style>