<script context="module">
    import { auth, from, fromBucket } from '$lib/supabase';
    export async function load({ params, fetch, session, stuff }) {
        try {
            let menu_info =  await from('menu').select().eq('type', params.type).single();
            console.log(params, menu_info.data);
            return {props:menu_info.data};
        } catch (error){
            console.log(error);
            return { props:{type:false}};
        }    
    }
</script>

<script>
    export let type;
    export let price;
    export let starters;
</script>
{#if type}
    <h1>This is the {type} menu.</h1>
    <h2>Price of : {price} euros</h2>


    <h3>Starters</h3>
    <ol>
    {#each starters as dish}
        <li>{dish}</li>
    {/each}    
    </ol>
{:else}
    <h1>This menu is not available.</h1>
{/if}

