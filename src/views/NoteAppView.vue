<template>
	<main class="container">
		<header class="flex items-center justify-between py-4">
			<h2 class="text-6xl font-bold">Notes</h2>

			<button type="button" class="h-16 w-16 rounded-full bg-slate-900 text-4xl text-white" @click="isModalOpen = true">+</button>
		</header>

		<section class="grid grid-cols-6 justify-items-center gap-4 gap-y-8 py-4">
			<div class="flex h-56 w-56 flex-col justify-between rounded-2xl p-4 shadow-lg" :style="{ backgroundColor: note.bgColor }" v-for="note in notes" :key="note.id">
				<p class="text-sm">{{ note.content }}</p>

				<p class="text-sm font-bold">{{ note.date }}</p>
			</div>
		</section>
	</main>

	<div v-show="isModalOpen" class="fixed inset-0 z-10 grid h-screen w-screen place-items-center bg-black/40">
		<div class="relative flex w-6/12 flex-col gap-4 rounded-lg bg-white p-4">
			<textarea v-model.trim="newNote" class="h-56 w-full rounded-lg border-2 border-blue-500 p-4 focus:outline-none" name="note"></textarea>

			<p class="text-xs text-red-600" v-if="noteError">{{ noteError }}</p>

			<div class="flex items-center justify-between">
				<button @click="closeModal()" type="button" class="rounded-lg bg-slate-900 py-3 px-4 text-xl font-bold text-white">Close</button>

				<button type="button" @click="addNote()" :class="['rounded-lg py-3 px-4 text-xl font-bold', newNote == '' ? 'cursor-not-allowed bg-gray-300 text-gray-700' : 'bg-slate-900 text-white']">Add Note</button>
			</div>
		</div>
	</div>
</template>

<script setup>
	import { ref } from 'vue';

	const isModalOpen = ref(false);

	const newNote = ref('');
	const noteError = ref('');

	const getRandomColor = () => {
		return `hsl(${Math.random() * 360}, 100%, 75%)`;
	};

	const addNote = () => {
		if (!newNote.value || !newNote.value.trim) {
			noteError.value = '* Note Cannot be Empty';
			return;
		}

		const date = new Date();

		notes.value.push({
			id: Math.random().toString(36),
			content: newNote.value,
			date: `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()}`,
			bgColor: getRandomColor(),
		});

		newNote.value = '';
		noteError.value = '';
		isModalOpen.value = false;
	};

	const closeModal = () => {
		noteError.value = '';
		isModalOpen.value = false;
	};

	const notes = ref([
		{
			id: 1,
			content: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sint alias modi repudiandae necessitatibus velit accusamus. Fuga earum ratione iure perspiciatis!',
			date: '2022/12/20',
			bgColor: 'hsl(254.24292366578797, 100%, 75%)',
		},
		{
			id: 2,
			content: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sint alias modi repudiandae necessitatibus velit accusamus. Fuga earum ratione iure perspiciatis!',
			date: '2022/12/20',
			bgColor: 'hsl(10.97468569202615, 100%, 75%)',
		},
	]);
</script>
