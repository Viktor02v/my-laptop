<script setup>
import { ref, onMounted, reactive, watch } from 'vue'
import axios from 'axios';
import Header from './components/Header.vue';
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])

const filters = reactive({
	sortBy: 'title',
	searchQuery: '',
})

const onChangeSortBy = (e) => {
	filters.sortBy = e.target.value
}

const onChangeSearch = (e) => {
	filters.searchQuery = e.target.value
}

const addToFavorite = async (item) => {
	try {
		if(!item.isFavorite) {
			const obj = {
				pruductId: item.id
			}
			item.isFavorite= true
			const {data} = await axios.post('https://54585bc361381db8.mokky.dev/favorites', obj)
			item.favoriteId = data.id
		} else{
			item.isFavorite= false
			await axios.delete(`https://54585bc361381db8.mokky.dev/favorites/${item.favoriteId}`)
			item.favoriteId = null
		}
	} catch (e) {
		console.log(e)
	}
}

const fetchFavorites = async () => {
	try {
		const { data: favorites } = await axios.get('https://54585bc361381db8.mokky.dev/favorites')
		items.value = items.value.map((item) => {
			const favorite = favorites.find((favorite) => favorite.productId === item.id);

			if (!favorite) {
				return item;
			}

			return {
				...item,
				isFavorite: true,
				favoriteId: favorite.id,

			}
		});
	} catch (e) {
		console.log(e)
	}
}

const fetchItems = async () => {
	try {
		const params = {
			sortBy: filters.sortBy
		}
		if (filters.searchQuery) {
			params.title = `*${filters.searchQuery}*`
		}

		const { data } = await axios.get('https://54585bc361381db8.mokky.dev/items', {
			params
		})
		items.value = data.map(obj => ({
			...obj,
			isFavorite: false,
			isAdded: false,
			favoriteId: null
		}))
	} catch (e) {
		console.log(e)
	}
}

onMounted(async () => {
	await fetchItems();
	await fetchFavorites();
});

watch(filters, fetchItems);


</script>
<template>
	<!-- <Drawer /> -->
	<div class="bg-indigo-200 mx-auto w-4/5 h-full rounded-3xl">
		<Header />
		<div class="m-10">
			<div class="flex justify-between items-center">
				<h2 class="text-3xl mb-10">All laptops</h2>
				<div class="flex gap-3">
					<select @change="onChangeSortBy"
						class=" bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block  px-3 py-2 "
						name="" id="">
						<option value="name">By name</option>
						<option value="price">By price(cheap)</option>
						<option value="-price">By price(expensive)</option>
					</select>
					<div class="relative flex items-center">
						<input @change="onChangeSearch"
							class=" bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full pl-10 py-2.5 z-10"
							type="text" placeholder="Search">
						<img class="absolute top- left-4" src="/search.svg" alt="">
					</div>
				</div>
			</div>
			<div>
				<CardList :items="items" @addToFavorite="addToFavorite" />
			</div>
		</div>
	</div>
</template>
<style></style>