<script lang="ts">
    import { goto } from '$app/navigation';

    export let data;

    let pageData = data;
    console.log(pageData);

    // layout.ts wraps supabase object and also page.ts - basically wraps twice
    let supabase = pageData.supabase.supabase;

    let loggedIn: Boolean = false;
    let username: string = '';
    let password: string = '';

    const login = async () => {
        const { data, error } = await supabase.auth.signInWithPassword({
            email: username,
            password: password,
        });

        if (error) {
            console.error('Error logging in:', error.message);
            loggedIn = false;
        } else {
            console.log('Logged in:', data.user.email);
            loggedIn = true;
            goto('/AuthorisedPage');
        }
    }

</script>

<div>
    <h1>Login</h1>
        <input type="text" placeholder="Username" bind:value={username} />
        <input type="password" placeholder="Password" bind:value={password} />
        <button type="submit" on:click={login}>Login</button>
</div>
{#if !loggedIn}
    <h4>Please provide login details</h4>
{/if}