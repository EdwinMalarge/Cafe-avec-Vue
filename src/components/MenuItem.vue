<template>
    <div class="menu-item">
        <img class="menu-item__image" :src="image.source" :alt="image.alt" />
        <div>
            <h3>{{ name }}</h3>
            <p> Prix : {{ generatedPrice }}
                <span v-if="onSale">(10% de réduction!)</span>
            </p>
            <p v-if="inStock">En stock</p>
            <p v-else>En rupture de stock</p>
            <div>
                <label for="add-item-quantity">Quantité :</label>
                <input id="add-item-quantity" v-model="quantity" type="number" />
                <button @click="updateShoppingCart(quantity)"> Ajouter au panier d'achat </button>
            </div>
        </div>
    </div>
</template>


<script>
import {mapActions} from "vuex"
export default {
    name: "MenuItem",

    props: {
        image: {
            type: Object,
            required: true
        },
        inStock: {
            type: Boolean,
            required: true
        },
        price: {
            type: Number,
            required: true
        },
        modelValue: {
            type: Number,
            default: 1
        },
        name: {
            type: String,
            required: true
        },
    },

    methods: {
        ...mapActions (["updateShoppingCart"])
    },

    data() {
        return {
            onSale: false
        }
    },
    computed: {
        generatedPrice() {
            if (this.onSale) {
                return (this.price * 0.9).toFixed(2)
            } else {
                return this.price
            }
        }
    },
    beforeMount() {
        const today = new Date().getDate()

        if (today % 2 === 0) {
            this.onSale = true
        }
    }
}
</script>

<style lang="scss">
.menu-item {
    display: flex;
    width: 500px;
    justify-content: space-between;
    margin-bottom: 30px;

    &__image {
        max-width: 300px;
    }
}
</style>