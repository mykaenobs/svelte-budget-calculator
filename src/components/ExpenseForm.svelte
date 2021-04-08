<script>
    import { getContext } from 'svelte';
    import SectionTitle from './Title.svelte';
    
    // this is how you export as props
    export let name = '', amount = null;
    
    // for my checking if editing
    export let isEditing, hideForm;

    $: isEmpty = !name || !amount;

    let { addExpense, editExpense } = getContext('state');
    const handleSubmit = () => {
        if (!isEditing)
            addExpense({ name, amount });
        else
            editExpense({ name, amount });

        name = "", amount = null;
        hideForm();
    };
</script>

<section class="form">
    <!-- The 'preventDefault' is add here it is svelte thing and is called modifier after the '|' -->
    <SectionTitle title="Add Expenses" />
    <form class="expense-form" on:submit|preventDefault={handleSubmit}>
        <div class="form-control">
            <label for="name">Name</label>
            <input type="text" id="name" bind:value={name} />
        </div>
        <div class="form-control">
            <label for="amount">amount</label>
            <input type="number" id="amount" bind:value={amount} />
        </div>
        <!-- Displaying if the fields are empty -->
        {#if isEmpty}
            <p class="form-empty">Please fill out all form fields</p>
        {/if}
        <button type="submit" class="btn btn-block" class:disabled={isEmpty} disabled={isEmpty}>
            <!-- Checking if isEditing or not -->
            {#if !isEditing} Add Expense {:else} Edit Expense {/if}
        </button>
        <!-- close buttton -->
        <button type="button" class="close-btn" on:click={hideForm}>
            <i class="fas fa-times" /> 
            close
        </button>
    </form>
</section>