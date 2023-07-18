<script>
import { store } from '../store.js';
import axios from "axios";

export default{
    data(){
        return{
            archetypeNumber: 0,
            store
        }

    },
    methods:{
        archetypesSelectTotal(){
            axios
			.get('https://db.ygoprodeck.com/api/v7/cardinfo.php')
			.then(response => {
				this.archetypeNumber = '';
				for (let i = 0; i < response.data.data.length; i++) {
					const card = response.data.data[i];
                    console.log(store.classSelect, 'class select')
					if (card.archetype == store.classSelect ) {
						this.archetypeNumber ++;
                        console.log('entrato')
					}
				}
				console.log('tot archetype', this.archetypeNumber)
		});
        }
    },
    mounted(){
        
    }
}

</script>

<template>

    <div>
        <p>
            The total of <span> {{ store.classSelect }} </span> are: <span> {{ archetypeNumber }} </span>
        </p>
    </div>

</template>

<style lang="scss" scoped>
div{
    display: inline-block;
    color: white;
    margin-left: 10px;

    p{
        display: inline-block;
        margin-right: 20px;
    }
    span{
        color: red;
    }
    
}
</style>