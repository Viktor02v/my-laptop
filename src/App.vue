<script setup>
import { ref, onMounted } from 'vue'
import  axios from 'axios';
import Header from './components/Header.vue';
import CardList from './components/CardList.vue'
import Draver from './components/Draver.vue'
const items = ref([])

onMounted(async () => {
	try{
		const { data } = await axios.get('https://54585bc361381db8.mokky.dev/items')
		items.value = data
	} catch (e) {
		console.log(e)
	}
})
</script>
<template>
	<Draver />
	<div class="bg-indigo-200 mx-auto w-4/5 h-full rounded-3xl">
		<Header />
		<div class="m-10">
			<h2 class="text-3xl mb-10">All laptops</h2>
			<div>
				<CardList :items="items" />
			</div>
		</div>
	</div>
</template>
<style></style>