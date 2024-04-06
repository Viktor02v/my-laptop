<script setup>
defineProps({
	title: String,
	price: Number,
	imageUrl: String,
	isAdded: Boolean,
	isFavorite: Boolean,
})
</script>
<template>
		<div class="flex flex-col justify-center relative bg-white py-10  px-5 rounded-2xl cursor-pointer hover:shadow-2xl transition hover:-translate-y-2  ">
			<img class="absolute top-4 left-5" :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="">
			<img :src="imageUrl" alt="mackBook">
			<p class="mt-5">{{title}}</p>
			<div class="mt-1 block">
				<span class="text-gray-500 block">Price:</span>
				<b> {{price}}UAN</b>
			</div>
			<img class="absolute bottom-4 right-5" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="">
		</div>
</template>
