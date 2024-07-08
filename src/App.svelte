<script>
import Router from "svelte-spa-router";

import Footer from "./lib/footer.svelte";
import Header from "./lib/header.svelte";
import Home from "./lib/home.svelte";
import Login from "./lib/login.svelte";
import Register from "./lib/register.svelte";
import Space from "./lib/space.svelte";
import Spaces from "./lib/spaceRoom.svelte";
import Stats from "./lib/stats.svelte";


const routes = {
  "/": Home,
  "/login": Login,
 "/spaces": Spaces,
 "/register": Register,
 '/space/:id' : Space,
};

</script>


<Header/>

<Router {routes}/>

<Stats/>