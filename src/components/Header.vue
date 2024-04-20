<script setup>
defineProps({
	totalPrice: Number,
})

const emit = defineEmits(['cartOpen'])
</script>
<template>
	<header class="flex justify-between p-10 border-b border-zinc-500	 mt-5 ">
		<router-link to="/">
			<div class="flex items-center ">
				<img src="/logo-laptop-particular.png" alt="logo">
				<div class="ml-5">
					<h1 class="font-bold">YOUR LAPTOP</h1>
					<p class="text-sm text-gray-500">The shop of your gaming dreams</p>
				</div>
			</div>
		</router-link>

		<div class="cursor-pointer">
			<div class="flex mx-10">
				<div @click=" emit('cartOpen')" class="flex items-center ">
					<img src="/cart.svg" alt="cart">
					<p class="ml-2  text-gray-600  hover:text-gray-900 hover:underline">{{ totalPrice }} UAN</p>
				</div>


				<div class="flex items-center ml-5 mr-5">
					<img src="/heart.svg" alt="like">
					<router-link to="/favorites">
						<p class="ml-2  text-gray-600 hover:text-gray-900 hover:underline">Tab</p>
					</router-link>
				</div>

				<div class="flex items-center">
					<img src="/profile.svg" alt="profile">
					<p class="ml-2  text-gray-600 hover:text-gray-900 hover:underline ">Profile</p>
				</div>
			</div>
		</div>
	</header>
</template>