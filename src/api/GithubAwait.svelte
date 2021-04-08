<script>
    const getUsers = async () => {
        const res = await fetch('https://api.github.com/users');
        return await res.json();
    };
</script>

<style>
    h2 {
        text-align: center;
    }
</style>

<section>
{#await getUsers()}
    <!-- promise is pending -->
    <h2>Loading...</h2>
{:then users}
    <!-- promise was fulfilled -->
    {#each users as user}
        <article class="user">
            <img src={user.avatar_url} alt={user.login}>
            <div class="user-info">
                <h3>User: {user.login}</h3>
                <a href={user.html_url} class="btn btn-primary" target="_blank">Github Url</a>
            </div>
        </article>
    {/each}
{:catch error}
    <!-- Promise was rejected -->
    <p>Something went wrong: {error.message}</p>
{/await}

</section>

