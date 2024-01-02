<template>
    <div class="products-container">
        <products-filters
            v-model:userSelectedSellers.check="selectedSellers"
            v-model:userSelectedBrand.check="selectedBrand"
            :products-list="products"
        />
        <products-list :products="filteredProducts" />
    </div>
</template>

<script>
import ProductsFilters from './ProductsFilters.vue'
import ProductsList from './ProductsList.vue'
export default {
    name: 'ProductsAndFilterManager',
    components: { ProductsFilters, ProductsList },
    data() {
        return {
            selectedSellers: [],
            selectedBrand: [],
        }
    },
    props: {
        products: {
            type: Array,
            default: () => [],
        },
    },
    computed: {
        filteredProducts() {
            console.log(this.selectedBrand)
            let prodList
            if (this.selectedSellers.length === 0 || this.selectedSellers.length == 2) {
                prodList = this.products
            } else {
                prodList = this.filterBySellers()
            }
            if (this.selectedBrand.length >= 1) {
                prodList = prodList.filter((prodData) => this.selectedBrand.includes(prodData.brand))
            }
            return prodList
        },
    },
    methods: {
        filterBySellers() {
            return this.products.filter((prodData) => {
                if (prodData.seller.toLowerCase() === this.selectedSellers[0]) return true
                else {
                    if (prodData.seller.toLowerCase() === 'rozetka') return false
                    else if (this.selectedSellers[0] === 'rozetka') return false
                    else return true
                }
            })
        },
    },
}
</script>

<style lang="scss" scoped></style>
