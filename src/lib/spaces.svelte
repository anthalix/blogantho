<script>

import { link } from "svelte-spa-router"


const get_space = async () => {
    
    

    const response = await fetch( "http://0.0.0.0:8055/items/Space" );
    
      const result = await response.json();
    
      return result.data;
    
}

</script>

{#await get_space()}

<p> chargement des espaces</p>
    
{:then data} 
    
{#each data as space }
    
<div class="space1" style="background-image: url({space.photoUrl});">
    <aside>
        <h2 class="title_space">{space.name}</h2>
        <p>Capacité : {space.size}pers</p><br>
        <p> Equipements:<br>{space.equipment} </p><br>
        <p>Prix:{space.price}€/jour</p><br>
        <p>{space.location}</p><br>
        {#if space.availibility == 1}
        <p>disponible</p>
        {:else}
         <p>reservé</p>  
         {/if}
        <a href="/space/{space.id}" use:link>Commenter</a>
    </aside>
</div>

{/each}

{/await}
