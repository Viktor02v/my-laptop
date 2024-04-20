<template>
	<div class="flex justify-between items-center">
		<h2 class="text-3xl mb-10">All laptops</h2>
		<div v-auto-animate class="flex gap-3">
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
				<img class=" z-20 absolute  top-3 left-4" src="/search.svg" alt="">
			</div>
		</div>
	</div>
	<div>
		<CardList v-auto-animate :items="items" @addToFavorite="addToFavorite" @onClickAddPlus="onClickAddPlus" />
	</div>
	
</template>

<script setup>
import { reactive, watch, ref, onMounted } from 'vue'
import axios from 'axios'
import { inject } from 'vue'
import debounce from 'lodash.debounce' 

import CardList from '../components/CardList.vue'


const { onClickAddPlus, cart } = inject('cart');

const items = ref([])
// For Filters : START
const filters = reactive({
	sortBy: 'title',
	searchQuery: '',
})

const onChangeSortBy = (e) => {
	filters.sortBy = e.target.value
}

const onChangeSearch = debounce((e) => {
	filters.searchQuery = e.target.value
}, 500)
// For Filters : END


// For Favorites : START
const addToFavorite = async (item) => {
	try {
		if (!item.isFavorite) {
			const obj = {
				item_id: item.id,
				
			}
			item.isFavorite = true
			const { data } = await axios.post('https://54585bc361381db8.mokky.dev/favorites', obj)
			item.favoriteId = data.id
		} else {
			item.isFavorite = false
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
			const favorite = favorites.find((favorite) => favorite.item_id === item.id);

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
// For Favorites : END


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
		console.log(items.value)
	} catch (e) {
		console.log(e)
	}
}

onMounted(async () => {
	const localCart = localStorage.getItem('cart');
	cart.value = localCart ? JSON.parse(localCart) : [];

	await fetchItems();
	await fetchFavorites();

	items.value = items.value.map((item) => ({
		...item,
		isAdded:  cart.value.some((cartItem) => cartItem.id === item.id),
	}))
});

watch(filters, fetchItems);

watch(cart, () => {
	items.value = items.value.map((item) => ({
		...item,
		isAdded: false,
		isFavorite: false
	}))
});
</script>