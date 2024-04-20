<template>
	<CardList v-auto-animate :items="favorites" is-favorites />
</template>

<script setup>
import {onMounted, ref} from 'vue';
import axios from 'axios';

import CardList from '../components/CardList.vue';

const favorites = ref([])

onMounted(async () => {
	try {
		const { data } = await axios.get('https://54585bc361381db8.mokky.dev/favorites?_relations=items')
		favorites.value = data.map((obj)=> obj.item )

	} catch (e) {
		console.log(e)
	}
})
</script>