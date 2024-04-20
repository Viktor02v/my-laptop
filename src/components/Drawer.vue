<script setup>
import { ref, computed } from 'vue'
import { inject } from 'vue';
import axios from 'axios'

import DrawerHead from './DrawerHead.vue'
import CartListItem from './CartListItem.vue';
import InfoBlock from './InfoBlock.vue';

const props = defineProps({
	totalPrice: Number,
	vatPrice: Number,
})

// For Cart: START
const { cart, closeDrawer } = inject('cart');

const isCreating = ref(false);
const orderId = ref(null);

const createOrder = async () => {
	try {
		// if (cart.value.length > 0){

		const prop = {
			items: cart.value,
			totalPrice: props.totalPrice,
			vatPrice: props.vatPrice,
		}

		isCreating.value = true

		const { data } = await axios.post('https://54585bc361381db8.mokky.dev/orders', prop);

		cart.value = [];

		orderId.value = data.id;

		alert('Order created successfully ✅');

		return data;
	}
	// }
	catch (e) {
		console.log(e)
	} finally {
		isCreating.value = false;
	}
}

const cartIsEmpty = computed(() => cart.value.length === 0);

const buttonDissabled = computed(() => isCreating.value || cartIsEmpty.value);

// For Cart: END
</script>
<template>
	<div class="fixed bg-black/50 top-0 left-0 w-4/5 h-full z-20"> </div>
	<div class="fixed  bg-white w-1/4 h-full top-0 right-0 p-5 z-30">
		<div class="mb-5">
			<DrawerHead />
		</div>
		<!-- Info-Blocks: START -->
		<div v-if="!totalPrice || orderId" class="flex h-full items-center ">
			<InfoBlock v-if="!totalPrice && !orderId" :title="'Cart is empty'" :description="'Please add some product'"
				:image-url="'/package-icon.png'" />
			<InfoBlock v-if="orderId" :title="'Order is created'" :description="`Your order #${orderId} is created successfully`"
				:image-url="'/order-success-icon.png'" />
		</div>
		<!--Info-Blocks: END  -->
		<div v-else>

			<div>
				<CartListItem />
			</div>

			<div class="flex  flex-col gap-4 my-7">
				<div class="flex gap-2">
					<span>Total:</span>
					<div class="border-b flex-1 border-dashed"></div>
					<b>{{ totalPrice }} UAN</b>
				</div>
				<div class="flex gap-2">
					<span>Tax 5%:</span>
					<div class="border-b flex-1 border-dashed"></div>
					<b>{{ vat }} UAN</b>
				</div>
				<div>
					<button :disabled="buttonDissabled" @click="createOrder"
						class=" bg-sky-500 text-white w-full px-5 py-2 rounded-3xl hover:bg-sky-600 active:bg-sky-700 transition disabled:bg-slate-300">Make
						an order</button>
				</div>
			</div>
		</div>
	</div>
</template>
