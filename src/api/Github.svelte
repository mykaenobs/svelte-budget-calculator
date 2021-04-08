<script>
    import { onMount } from "svelte";
    let users = [];
    let isLoading = true;

    onMount(async () => {
        const res = await fetch('https://api.github.com/users');
        users = await res.json();
        isLoading = false;
    });
</script>

<style lang="scss">
    h2 {
        text-align: center;
    }
</style>

{#if isLoading}
    <h2>Loading...</h2>
{:else}
    <section>
        {#each users as user}
            <article class="user">
                <img src={user.avatar_url} alt={user.login}>
                <div class="user-info">
                    <h3>User: {user.login}</h3>
                    <a href={user.html_url} class="btn btn-primary" target="_blank">Github Url</a>
                </div>
            </article>
        {/each}
    </section>
{/if}