<script setup>
import Card from './Card.vue'


defineProps({
	items: Array,
})

const emit = defineEmits(['addToFavorite'])

</script>
<template>
	<div class="grid grid-cols-4 gap-5 pb-10">
		<Card v-for="item in items" :key="item.id" :title="item.title" :price="item.price" :imageUrl="item.imageUrl"
			:isFavorite="item.isFavorite" :onClickFavorite="() => emit('addToFavorite',item)" />
	</div>
</template>
