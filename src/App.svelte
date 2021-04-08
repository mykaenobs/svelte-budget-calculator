<script>
	// api github
	// import Github from './api/Github.svelte';
	// import GithubAwait from './api/GithubAwait.svelte';
	
	import { setContext, onMount, afterUpdate } from 'svelte';

	// Components
    import Navbar from './components/Navbar.svelte';
    import ExpensesList from './components/ExpensesList.svelte';
    import ExpenseForm from './components/ExpenseForm.svelte';
    import Totals from './components/Totals.svelte';
	import Modal from "./components/Modal.svelte";

	// Data
    // import expenseData from './expenses';

	// variables
    let expenses = [];
	let isFormOpen = false;

	// set edit variables
	let setName = '', setAmount = null, setId = null;

	// reactive
	$: isEditing = setId ? true : false;
	$: total = expenses.reduce((a, c) => { 
		return a += c.amount;
	}, 0);

	/********** Functions ***********/

	const showForm = () => isFormOpen = true;
	const hideForm = () => (isFormOpen = false, setName = '', setAmount = null, setId = null);

	// To remove an expense
    const removeExpense = (id) =>
        (expenses = expenses.filter((item) => item.id !== id));

	// To clear all expenses
	const clearExpenses = () => 
		(expenses = []);

	// To add expense
	const addExpense = ({ name, amount }) => {
		let expense = { id: Math.random() * Date.now(), name, amount };
		expenses = [expense, ...expenses];
	};

	// To set modified expense expense
	const setModifiedExpense = (id) => {
		let expense = expenses.find(item => item.id === id);
		setName = expense.name, setAmount = expense.amount, setId = expense.id;
		showForm();
	};

	// To edit expense
	const editExpense = ({ name, amount }) => {
		console.log({name, amount});
		expenses = expenses.map(item => {
			return item.id === setId ? { ...item, name, amount } : { ...item };
		});
		setName = '', setAmount = null, setId = null;
	};
	
	// App state
	setContext('state', {
		removeExpense, clearExpenses, addExpense, setModifiedExpense, editExpense
	});

	// Local storage to store the expenses
	const setLocalStorage = () =>
		(localStorage.setItem('expenses', JSON.stringify(expenses)));

	// Getting data ones the component mounts
	onMount(() => {
		expenses = localStorage.getItem('expenses')
			? JSON.parse(localStorage.getItem('expenses'))
			: [];
	});

	afterUpdate(() => setLocalStorage());

</script>

<header>
    <Navbar {showForm} />
</header>
<main class="content">
	{#if isFormOpen}
		<Modal>
			<ExpenseForm name={setName} amount={setAmount} {isEditing} {hideForm} />
		</Modal>
	{/if}
	<Totals title="Total Expenses" {total} />
    <ExpensesList {expenses} />
</main>
