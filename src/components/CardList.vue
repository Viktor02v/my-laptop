<script setup>
import Card from './Card.vue'


defineProps({
	items: Array,
})

</script>
<template>
	<div class="grid grid-cols-4 gap-5 pb-10">
		<Card v-for="item in items" :key="item.id" :title="item.title" :price="item.price" :imageUrl="item.imageUrl"
			:isAdded="false" :isFavorite="false" />
	</div>
</template>
