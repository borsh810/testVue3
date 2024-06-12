<script setup lang="ts">
import { ref, reactive, computed } from 'vue';

interface Input {
  id: number;
  value: string;
}

const inputs = reactive<Input[]>([{ id: 1, value: '' }, { id: 2, value: '' }, { id: 3, value: '' }]);
const nextId = ref<number>(4);
const searchText = ref<string>('');

const addInput = (): void => {
  if (inputs.length < 7) {
    inputs.push({ id: nextId.value++, value: '' });
  }
};

const removeInput = (id: number): void => {
  if (inputs.length > 1) {
    const index = inputs.findIndex((input: Input) => input.id === id);
    inputs.splice(index, 1);
  }
};

const countVowels = (str: string): number => (str.match(/[aeiou]/gi) || []).length;

const vowelCounts = computed<number[]>(() => inputs.map((input: Input) => countVowels(input.value)));

const highlight = (value: string): boolean => value.includes(searchText.value);

</script>

<template>
	<header>
		<div class="wrapper">
		</div>
	</header>

	<main>
		<form>
			<div class="form-group">
				<label for="search">Search</label>
				<input type="text" id="search" v-model="searchText" class="form-control"
					:class="{ highlight: inputs.some(input => highlight(input.value)) }">
			</div>
			<div class="form-group" v-for="(input, index) in inputs" :key="input.id">
				<label :for="'input' + input.id">Input {{ input.id }} (Vowels: {{ vowelCounts[index] }})</label>
				<input type="text" :id="'input' + input.id" v-model="input.value" class="form-control"
					:class="{ highlight: highlight(input.value) }">
				<button type="button" @click="removeInput(input.id)">Remove</button>
			</div>
			<button type="button" @click="addInput" :disabled="inputs.length >= 7">Add Input</button>
		</form>
	</main>
</template>

<style scoped>
.form-group {
	margin-bottom: 1rem;
}

.highlight {
	background-color: green;
}
</style>
