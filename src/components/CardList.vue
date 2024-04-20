<script setup>
import Card from './Card.vue'


defineProps({
	items: Array,
	isFavorites: Boolean,
})

const emit = defineEmits(['addToFavorite, onClickAddPlus'])

</script>
<template>
	<div class="grid grid-cols-4 gap-10 py-10">
		<Card v-for="item in items" :key="item.id" :title="item.title" :price="item.price" :imageUrl="item.imageUrl"
			:isFavorite="item.isFavorite"
			:isAdded="item.isAdded" 
			:onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
			:onClickAdd="isFavorites ? null : () => emit('onClickAddPlus', item)" />
	</div>
</template>
