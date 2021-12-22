<script lang="ts">
    import UserDetail from "./UserDetail.svelte"

    type User = {
        login: string
        avatar_url: string
        showDetails: boolean
    }
    async function getUsers(): Promise<User []> {
        const res = await fetch("https://api.github.com/users")
        const users = await res.json()

        console.log("the users", users);

        if (res.ok) {
            return users
        } else {
            throw new Error(users)
        }
    }

    $: allUsersPromise = getUsers()
</script>

<section class="w-1/2 m-auto border border-gray-400 p-8 rounded">
    {#await allUsersPromise then users}
        {#each users as user}
            <div class="m-4 cursor-pointer">
                <div class="flex">
                    <img class="rounded-full w-12" src={user.avatar_url} alt="avatar" />
                    <p class="my-auto font-semibold ml-2">{user.login}</p>
                </div>
            </div>

            {#if user.showDetail}
                <UserDetail
                    userLogin={user.login}
                    on:closeDetails={() => {
                        setTimeout(() => (user.showDetails = false))
                    }}
                />
            {/if}
        {/each}
    {/await}
</section>