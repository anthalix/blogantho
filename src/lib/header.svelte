<script>
// @ts-nocheck
 import logo from '../assets/coclockworking.png';

    let isOpen = false;
  
    function toggleMenu() {
      isOpen = !isOpen;
    }


import { link } from "svelte-spa-router"

    const nav_links = [
        {
            label: "page d'accueil du site",
            text: "Accueil",
            url: "/",
        },

        {
            label: "page de réservation",
            text: "Espaces",
            url: "/spaces",
        },

        {
            label: "Formulaire de connection",
            text: "Se connecter",
            url: "/login",
        },

        {
            label: "Formulaire d'inscription",
            text: "S'enregistrer",
            url: "/register",
        },
    ];
  

  </script>


<header>
    <ul class="menu">
        <li class="logo">
            <img
                src="{logo}"
                aria-label="accueil du site"
                alt="logo entreprise"
            />
            <span class="title_menu">Co'Clock Working</span>
        </li>
        <nav class="navbar {isOpen ? 'mobile-menu' : ''}">
            {#each nav_links as nav}
            <a role="menuitem" aria-label={nav.label} href={nav.url} use:link
                >{nav.text}</a
            >
        {/each}
        </nav>
        <button class="menu-burger" aria-label="toggle-menu"on:click={toggleMenu} >
            <i class="fa-solid fa-bars" ></i>
        </button>
    </ul>
</header>