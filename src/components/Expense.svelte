<script>
    import { getContext } from "svelte";
    import { blur, slide, scale, fade, fly } from 'svelte/transition';
    export let name = '', amount = 0, id;
    let isHidden = false;

    const displayAmount = () => 
        isHidden = !isHidden;

    let { removeExpense, setModifiedExpense } = getContext('state');

</script>

<article class="single-expense">
    <div class="expense-info">
        <h2>
            {name} 
            <button class="amount-btn" on:click={displayAmount}>
                <i class="fas fa-caret-down" />
            </button>
        </h2>
        {#if isHidden}
            <h4 transition:slide>Amount: ${amount}</h4>
        {/if}
    </div>
    <div class="expense-buttons">
        <button class="expense-btn edit-btn" on:click={() => setModifiedExpense(id)}>
            <i class="fas fa-pen" />
        </button>
        <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
            <i class="fas fa-trash" />
        </button>
    </div>
</article>