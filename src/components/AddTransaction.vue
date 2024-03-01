<template>
	<h3>Add new Transaction</h3>
	<form id="form" @submit.prevent="onSubmit">
		<div class="form-control">
			<label for="text">Text</label>
			<input type="text" id="text" placeholder="Rent"
			v-model="text">
		</div>
		<div class="form-control">
			<label for="amount">
				Amount: <br>
			</label>
			<input type="number" id="amount" placeholder="-R2500"
			v-model="amount">
		</div>
		<button class="btn">Add Transaction</button>
	</form>
</template>

<script setup>
	import {ref, computed} from 'vue'
	import { useToast } from 'vue-toastification';

	const text  = ref('')
	const amount  = ref('')

	const emit = defineEmits(['transactionSubmitted'])

	const toast = useToast();

	const onSubmit = () => {
		if (!text.value || !amount.value) {
			toast.error('Both fields must be filled');
			return
		}

		const transactionData = {
			text: text.value,
			amount: parseFloat(amount.value)
		}

		emit('transactionSubmitted', transactionData)

		text.value = '';
		amount.value = '';
		
	}
</script>
