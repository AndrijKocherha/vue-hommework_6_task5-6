<template>
    <div class="filters">
        <div class="filters__block" :class="{ 'is-empty': isSellerSelectionEmpty }">
            <h6 class="filters__title">Продавці 2</h6>
            <label class="filters__filter"
                ><input v-model="selectedSellers" value="rozetka" type="checkbox" />Rozetka
                <span>({{ rozetkaCount }})</span>
            </label>
            <label class="filters__filter"
                ><input v-model="selectedSellers" value="other-sellers" type="checkbox" />Інші продавці
                <span>({{ otherSellerCount }})</span>
            </label>
        </div>
        <div class="filters__block filters__block_brands" :class="{ 'is-empty': isBrandSelectionEmpty }">
            <h6 class="filters__title">Бренди</h6>
            <label v-for="[brandName, brandData] in brands" :key="brandData.id" class="filters__filter"
                ><input v-model="selectedBrand" :value="brandName" type="checkbox" />{{ brandName }}
                <span>({{ brandData.count }})</span>
            </label>
        </div>
    </div>
</template>

<script>
const defaultSeller = 'rozetka'
export default {
    name: 'ProductsFilters',
    data() {
        return {
            rozetkaCount: 0,
            otherSellerCount: 0,
            brands: new Map(),
            selectedSellersVal: [],
            selectedBrandsVal: [],
            isSellerSelectionEmpty: true,
            isBrandSelectionEmpty: true,
        }
    },
    props: {
        productsList: {
            type: Array,
            default: () => [],
        },
        userSelectedSellers: {
            type: Array,
        },
        userSelectedSellersModifiers: {
            default: () => ({}),
        },
        userSelectedBrand: {
            type: Array,
        },
        userSelectedBrandModifiers: {
            default: () => ({}),
        },
    },
    computed: {
        selectedSellers: {
            get() {
                return this.selectedSellersVal
            },
            set(newSeller) {
                this.selectedSellersVal = newSeller
                if (this.userSelectedSellersModifiers.check) {
                    this.isSellerSelectionEmpty = this.changeSelectedStatus(this.selectedSellersVal)
                }
                this.$emit('update:userSelectedSellers', newSeller)
            },
        },
        selectedBrand: {
            get() {
                return this.selectedBrandsVal
            },
            set(newBrands) {
                this.selectedBrandsVal = newBrands
                if (this.userSelectedBrandModifiers.check) {
                    this.isBrandSelectionEmpty = this.changeSelectedStatus(this.selectedBrand)
                }
                this.$emit('update:userSelectedBrand', newBrands)
            },
        },
    },
    methods: {
        setSellersAndBrands() {
            for (const prodData of this.productsList) {
                if (prodData.seller.toLowerCase() === defaultSeller) this.rozetkaCount++
                else this.otherSellerCount++
                this.setBrands(prodData)
            }
        },
        changeSelectedStatus(selectedFilters) {
            return !(selectedFilters.length >= 1)
        },
        setBrands(prodData) {
            let brand = this.brands.get(prodData.brand)
            if (brand) brand.count++
            else {
                this.brands.set(prodData.brand, {
                    count: 1,
                    id: prodData.id,
                })
            }
        },
    },
    created() {
        this.setSellersAndBrands()
    },
}
</script>

<style lang="scss" scoped>
.is-empty {
    border: 1px solid green;
}
</style>
