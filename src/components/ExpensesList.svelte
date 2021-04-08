<script>
    import { getContext } from "svelte";
    import { fly } from 'svelte/transition';
    import { flip } from 'svelte/animate';
    // component
    import SectionTitle from './Title.svelte';
    import Expense from "./Expense.svelte";
    // variables
    export let expenses = [];
    let { clearExpenses } = getContext('state');
</script>

<section>
    <SectionTitle title="Expense List" />
    <ul>
        {#each expenses as expense, index (expense.id)}
            <li transition:fly={{x: 200, delay: index * 500}} animate:flip={{duration: 350}}>
                <Expense {...expense} />
            </li>
        {:else}
            <h2>No Expenses Add To The List</h2>
        {/each}
    </ul>
    <button class="btn btn-primary btn-block" type="button" on:click={clearExpenses}>
        Clear Expenses
    </button>
</section>