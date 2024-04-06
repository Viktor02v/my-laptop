<script setup>
import DraverHead from './DraverHead.vue'
import CartListItem from './CartListItem.vue';
</script>
<template>
	<div class="fixed bg-black/50 top-0 left-0 w-4/5 h-full z-10">	</div>
		<div class="fixed bg-white w-1/5 h-full top-0 right-0 p-5">
			<div class="mb-5">
				<DraverHead />
			</div>

			<CartListItem />

			<div class="flex flex-col gap-4 my-7">
				<div class="flex gap-2">
					<span>Total:</span>
					<div class="border-b flex-1 border-dashed"></div>
					<b>1205 UAN</b>
				</div>
				<div class="flex gap-2">
					<span>Tax 5%:</span>
					<div class="border-b flex-1 border-dashed"></div>
					<b>105 UAN</b>
				</div>
				<div>
				<button 
				disabled=""
					class=" bg-sky-500 text-white w-full px-5 py-2 rounded-3xl hover:bg-sky-600 active:bg-sky-700 transition disabled:bg-slate-300">Make
					an order</button>
			</div>
			</div>
		</div>
</template>