<script>
    import Login from "./login.svelte";
    import { onMount } from "svelte";
    import { push } from "svelte-spa-router";
    import { getContext } from 'svelte';

    export let params = {};
    const space_id = params.id;

    let currentPath = "";

  
    const router = getContext('svelte-spa-router');
    let token  = localStorage.getItem("token")  ;



    let commentTitle=  ""  ;
    let commentRate=  ""   ;
    let commentDesc=  ""   ;

    let comments =[];


onMount(() => {
        token = localStorage.getItem("token");
        currentPath = router; 
    });

function handleLogin(event) {
        token = event.detail.token;
        push(currentPath);
    }   

const load_comments = async (comments) => {
        // Récupérer le token depuis localStorage
        const token  = localStorage.getItem("token")  ;

        const resa_ids = await get_resa_ids(token, space_id);
        comments = await get_comments(token, resa_ids);

        return comments;
    };

const get_space = async (id) => {
        const response = await fetch("http://0.0.0.0:8055/items/Space/" + id);

        const result = await response.json();

        return result.data;
    };

const get_resa_ids = async (token, id) => {
        let endpoint = "http://0.0.0.0:8055/items/Booking?";
        endpoint += "filter[space_id][_eq]=" + id;
        endpoint += "&fields=space_id";

        const response = await fetch(endpoint, {
            headers: {
                Authorization: "Bearer " + token
            },
        });
        const result = await response.json();
        let ids = [];

        result.data.forEach((item) => {
            ids.push(item.space_id);
        });
        return ids;
    };

const get_comments = async (token, resa_ids) => {
        let endpoint = "http://0.0.0.0:8055/items/Comment?";
        endpoint += "filter[space_id][_in]=" + resa_ids.join(",");

        const response = await fetch(endpoint, {
            headers: {
                Authorization: "Bearer " + token
            },
        });
        const result = await response.json();
        return result.data;
    };





const handleSubmitForm =async (event) =>{

        event.preventDefault();
        const new_comment = await post_comment();


        commentDesc ="";
        commentTitle="";

        comments.push(new_comment);
         comments=[... comments];
}
const post_comment = async ()=>{

       const response = await fetch ( "http://0.0.0.0:8055/items/Comment", {
           method : "POST",
           headers :{
            "content-type": "application/json",
            "authorization" : "Bearer " + token
           },
           body: JSON.stringify({
            title : commentTitle,
            rate : commentRate,
            description: commentDesc,
            space_id: space_id

           })
           })
    const json= await response.json();
    return json.data;

     }



</script>

    {#await get_space(space_id)}
        <p>Récupération des données de l'espace...</p>
    {:then data}
        <main aria-labelledby="espace Bora">
            <h1>{data.name}</h1>
            <img class="open_space_img" src={data.photoUrl} alt="mon espace" />

            <section class="space" aria-label="information de l'espace">
                <p>
                    <b>Capacité :</b>
                    {data.size} personnes
                </p>
                <p>
                    <b>Equipements :</b>
                    {data.equipment}
                </p>
                <p>
                    <b>Prix :</b>
                    {data.price}€ la journée
                </p>
                <p>
                    <b>Localisation</b>
                    {data.location}
                </p>

                {#if data.availibility == 1}
                    <p>disponible</p>
                {:else}
                    <p>réservé</p>
                {/if}
            </section>

            <h3 id="comment_title">Commentaires</h3>
            <br />

        </main>
    {/await}



            {#if !token} 
            <h2>Vous devez être connecter</h2>
           <Login on:submit={handleLogin} redirectUrl={currentPath} />
            {:else}

            {#await load_comments(comments)}
                <p>En attente</p>
            {:then list}
                {#each list as comment}
                    <section
                        class="comments"
                        aria-labelledby="Commentaires sur l'espace"
                    >
                        <div aria-labelledby="Comment-1" class="comment-1">
                            <h4 id="comment-1">{comment.title}</h4>
                            <i><em>Posté le {comment.date_created}</em></i>
                            <br /><br />
                            <p>{comment.description}</p><br>
                            <p><em>Note : {comment.rate}</em></p>
                        </div>
                    </section>
                {/each}
            {/await}


            <form  on:submit={handleSubmitForm} class=" form_comment" action="/" aria-labelledby="form-title">
                <h3 id=" form-title">Ajouter un commentaire</h3>


                <label for="title">Titre</label>
                <input name="title" id="title"   bind:value={commentTitle}/>
                <label for="rate">Note</label>
                <input name="rate" id="rate" type="number" max="5" min="1" bind:value={commentRate}/>

                <label for="comment">texte du commentaire</label>
                <textarea name="comment" id="comment" cols="50" rows="10" bind:value={commentDesc}></textarea>

                <input type="submit" value="commenter" />
            </form>
        
    
            {/if}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        