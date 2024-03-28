<script>
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    export let data;
    let user = '';

    console.log("AP data -> ", data)

    // refer to homepage +page.svelte comment
    let supabase = data.supabase.supabase;

    onMount(async () => {
        await getUserEmail();
        // if (!user) {
        //     console.log('No user logged in');
        //     goto('/');
        // }
    });

    const getUserEmail = async () => {
        const { data, error } = await supabase.auth.getUser();

        if (error) {
            console.error('Error getting user:', error.message);
        } else {
            console.log('User:', data.user.email);
            user = data.user.email || 'No user';
        }
    }

    const logout = async () => {
        const { error } = await supabase.auth.signOut();

        if (error) {
            console.error('Error logging out:', error.message);
        } else {
            console.log('Logged out');
            user = '';
        }
    }

</script>

{#if user}
    <h3>Authorised Page - No non-logins allowed!</h3>

    <button on:click={() => getUserEmail()}>getUser</button>
    <button on:click={() => logout()}>logout</button>

    {#if user}
        <p>{user}</p>
    {/if}
{:else}
    <h2>Fuck off!  You're not logged in!!</h2>
{/if}
