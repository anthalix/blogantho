    <script>
        import { push } from "svelte-spa-router";

        import {createEventDispatcher} from 'svelte';
         
        const dispatch = createEventDispatcher();
        export let redirectUrl = '/'; 
        
        let email;
        let pwd;
        
        
        const handleSubmit = async (event)=>{
        
        event.preventDefault();
        
        const token = await login();
        
        localStorage.setItem('token', token);

        dispatch('submit', {token});

        push(redirectUrl);
        
        
        }
        const login = async () =>{
        
            const response = await fetch("http://0.0.0.0:8055/auth/login",{
        
                method: "POST",
                headers :{
        
                      "content-type" :"application/json"
                        },
                        body: JSON.stringify({
                        "email": email,
                        "password": pwd
                    })
        
            })
        
                 const json = await response.json();
                 return json.data.access_token;
        
        }
        
        
        
        </script>

    <h1>Se connecter</h1>

    <form on:submit={handleSubmit} aria-label="  informations connection">

    
    <label for="email">Email</label>
    <input  type="email"  id="email" name="email" placeholder="Email" bind:value={email}/>
    
    <label   for="password">Mot de passe</label>
    <input type="password" id ="password"  name="password" placeholder="******" bind:value={pwd}/>

    <input type="submit" class="button" value="Se connecter"/>


    </form>




