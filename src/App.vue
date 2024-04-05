<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'

const items = ref([])

defineProps({
	title: String,
	price:Number,
	imageUrl:String,
})
onMounted(async () => {
	try {
		const {data} =  axios.get('https://54585bc361381db8.mokky.dev/items');
		items.value = data;
		console.log(data)
	} catch (error) {
		console.log(error)
	}

})

</script>
<template>
	<div class="bg-indigo-200 mx-auto w-4/5 h-full">
		<Header />
		<div class="m-10">
			<h2 class="text-3xl mb-10">All laptops</h2>
			<div class="grid grid-cols-4 gap-5">
				<div class="bg-white py-10  px-5 rounded-2xl">
					<img src="/laptops/macBook.jpg" alt="">
					<p class="mt-5">MacBook Air 13</p>
					<div class="mt-1 block">
						<span class="text-gray-500 block">Price:</span>
						<b> {{price}} UAN</b>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<style></style>