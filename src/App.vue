<template>
  <Header />

  <div class="container">
  	<Balance  :total="total"/>
  	<IncomeExpenses :income="+income" :expenses="+expenses" />
  	<TransactionList :transactions="transactions" 
  	@transactionDeleted="handletransactionDeleted"/>
  	<AddTransaction @transactionSubmitted="handTransactionSubmitted" />
  </div>
</template>

<script setup>
	
	// Components
	import Header from './components/Header.vue'
	import Balance from './components/Balance.vue'
	import IncomeExpenses from './components/IncomeExpense.vue'
	import TransactionList from './components/TransactionList.vue'
	import AddTransaction from './components/AddTransaction.vue'


	import {ref, computed, onMounted} from 'vue'
	import { useToast } from 'vue-toastification';

	const toast = useToast();

	const transactions = ref([]);

	onMounted(() => {
		const savedTransaction = JSON.parse(localStorage.getItem('transactions'));

		if (savedTransaction)  {
			transactions.value = savedTransaction;
		}
	})

	// Get Total
	const total = computed(() => {
		return transactions.value.reduce((acc, transaction)=> {
			return acc + transaction.amount;
		}, 0);
	});

	// Get Income
	const income = computed(() => {
		return transactions.value.
		filter((transaction) => transaction.amount > 0)
		.reduce((acc, transaction)=> {
			return acc + transaction.amount;
		}, 0)
		.toFixed(2);
	});


	//Get expenses
	const expenses = computed(() => {
		return transactions.value.
		filter((transaction) => transaction.amount < 0)
		.reduce((acc, transaction)=> {
			return acc + transaction.amount;
		}, 0)
		.toFixed(2);
	});

	// Add transaction
	const handTransactionSubmitted = (transactionData) => {
		transactions.value.push({
			id: generateUniqueId(),
			text: transactionData.text,
			amount: transactionData.amount
		});

		saveTransactionToLS();

		toast.success('Transaction Added Successfully')
	};

	//Generate unique id
	const generateUniqueId = () => {
		return Math.floor(Math.random() * 1000000)
	}

	//delete Transaction
	const handletransactionDeleted = (id) => {
		transactions.value = transactions.value
		.filter((transaction) => transaction.id !== id);

		saveTransactionToLS();

		toast.success('Transaction deleted!');
	}

	// Save to localStorage
	const saveTransactionToLS = () =>
	localStorage.setItem('transactions', JSON.stringify(transactions.value));

</script>