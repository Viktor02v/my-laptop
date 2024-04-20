<script setup>
const props = defineProps({
	title: String,
	price: String,
	imageUrl: String,
	isAdded: Boolean,
	isFavorite: Boolean,
	onClickAdd: Function,
	onClickFavorite: Function
})

const visibleFavoriteButton = Boolean(props.onClickAdd)

</script>
<template>
	<div
		class="flex flex-col justify-center relative bg-white py-10  px-5 rounded-2xl cursor-pointer hover:shadow-2xl transition hover:-translate-y-2  ">
		<img v-if="visibleFavoriteButton" class="absolute top-4 left-5" @click="onClickFavorite"
			:src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="">
		<img  :src="imageUrl" alt="mackBook">
		<p class="mt-5">{{ title }}</p>
		<div class="mt-1 block">
			<span class="text-gray-500 block">Price:</span>
			<b> {{ price }}UAN</b>
		</div>
		<img v-if="visibleFavoriteButton" @click="onClickAdd" class="absolute bottom-4 right-5" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="">
	</div>
</template>
