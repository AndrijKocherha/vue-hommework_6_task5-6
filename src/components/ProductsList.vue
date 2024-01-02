<template>
    <div class="products-body">
        <div v-for="product in products" class="products-body__product product" :key="product.id">
            <div class="product__image"><img :src="product.imageSrc" alt="" /></div>
            <div class="product__descr">{{ product.title }}</div>
            <div class="product__price">{{ product.price }} <span> ₴</span></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductsList',
    props: {
        products: {
            type: Array,
            default: () => [],
        },
    },
}
</script>

<style lang="scss" scoped></style>
