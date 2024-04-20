<script setup>
defineProps({
	title: String,
	price: String,
	imageUrl: String,
	onClickRemove: Function,
})
</script>
<template>
				<div class=" flex justify-around items-center py-5 p-3 border mb-5 rounded-2xl hover:shadow-2xl hover:translate-x-2 transition hover:cursor-pointer">
				<img height="10" width="80" :src="imageUrl" :alt="title">
				<div class="flex flex-col overflow-auto ">
					<p>{{ title }}</p>
					<b>{{price}} UAN</b>
				</div>
				<img @click="onClickRemove" class="cursor-pointer hover:scale-110 transition hover:duration-100 hover:ease-in"  src="/close.svg" alt="">
			</div>
</template>