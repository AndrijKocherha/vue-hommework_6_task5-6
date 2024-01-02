<template>
    <products-and-filter-manager :products="products" />
    <credit-card-details />
</template>

<script>
import CreditCardDetails from './components/CreditCardDetails.vue'
import ProductsAndFilterManager from './components/ProductsAndFilterManager.vue'
import { products } from './constants/products'
export default {
    name: 'App',
    components: { ProductsAndFilterManager, CreditCardDetails },
    data() {
        return {
            products,
        }
    },
}
</script>

<style lang="scss">
@import './assets/style/index';
body {
    font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
}
</style>
