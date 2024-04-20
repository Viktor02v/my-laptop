<script setup>

import { ref, watch, provide, computed } from 'vue'
import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue'

// For Cart: START

const cart = ref([])

const drawerOpen = ref(false)

const cartOpen = () => {
	drawerOpen.value = true
}
const cartClose = () => {
	drawerOpen.value = false
}

const onClickAddPlus = (item) => {
	if (!item.isAdded) {
		addToCart(item);
	} else {
		removeFromCart(item);
	}
}

const addToCart = (item) => {
	cart.value.push(item)
	item.isAdded = true
}

const removeFromCart = (item) => {
	cart.value.splice(cart.value.indexOf(item), 1)
	item.isAdded = false
}

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round(totalPrice.value * 5) / 100)

// For Cart: END

watch(cart, () => {
	localStorage.setItem('cart', JSON.stringify(cart.value))
},
	{ deep: true }
)

provide('cart', {
	cart,
	onClickAddPlus,
	removeFromCart,
	cartOpen,
	cartClose,
})

</script>
<template>
	<Drawer  v-if="drawerOpen" :total-price="totalPrice" :vat-price="vatPrice" />

	<div class="bg-indigo-200 mx-auto w-4/5 h-full rounded-3xl">
		<Header :total-price="totalPrice" @cart-open="cartOpen" />

		<div class="m-10">
			<router-view></router-view>
		</div>

	</div>
</template>
<style></style>