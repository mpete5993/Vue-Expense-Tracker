<template>
	<div class="inc-exp-container">
		<div>
			<h4 style="color: #333;">Income</h4>
			<p id="money-plus" class="money plus">+R{{income}}  </p>
		</div>
		<div>
			<h4 style="color: #333;">Expense</h4>
			<p id="money-minus" class="money minus">R{{expenses}}</p>
		</div>
	</div>
</template>

<script setup> 

	import {defineProps} from 'vue';

	const props = defineProps({
		income: {
			type: Number,
			required: true
		},
		expenses: {
			type: Number,
			required: true
		}
	})
</script>
