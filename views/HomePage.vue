<template>
  <div v-for="(item, index) in products" :key="index">
    <center><fieldset>
        <h4>{{ item.title }}</h4>
        <h5>{{ item.price }} €</h5>
        <img v-bind:src="item.image">
        <br>
        <br>
        <button type="button" @click="openModal(index)">Voir plus d'infos</button>
        <div class="modal" v-if="openedModal" v-cloak>
            <div class="modal-content">
                <span class="close" @click="closeModal">X</span>
                <br>
                <div><i>{{ selectedProduct.description }}</i></div>
                <br>
                <br>
                <div class="stock">En stock : <span>{{ selectedProduct.rating.count }}</span></div>
            </div>
        </div>
    </fieldset></center>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
    data(){
        return{
            openedModal: false,
            selectedProduct: {}
        };
    },

    methods:{
        openModal(index) {
            this.selectedProduct = this.products[index];
            this.openedModal = !this.openedModal;
        },
        
        closeModal() {
            this.openedModal = !this.openedModal;
        }
    },

    computed:{
        // products(){ return this.$store.state.products }
        ...mapState(['products'])
    },

    mounted () {
        this.$store.dispatch('loadItems')
    },
}
</script>

<style>
fieldset {
    width: 30%;
    margin-bottom: 10px
}

img {
    width:150px
}

h5 {
    color: blue
}

.stock {
    font-weight: bold
}
span {
    color: red
}

.modal {
    display: flex; 
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5); 
    z-index: 1; 
    justify-content: center;
    align-items: center;
}

.modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
    max-width: 400px;
    width: 100%;
    position: relative;
}

.close {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
    font-size: 18px;
    color: #333;
    z-index: 2; 
}

[v-cloak] {
	display: none;
}

button {
    cursor: pointer
}
</style>