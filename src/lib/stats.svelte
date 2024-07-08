<script>
    import { onMount } from 'svelte';
    import { writable, derived } from 'svelte/store';

    // Store pour les données des espaces
    let spaces = writable([]);

    // Chargement initial des données des espaces
    onMount(async () => {
        try {
            const response = await fetch("http://0.0.0.0:8055/items/Space");
            const result = await response.json();

            spaces.set(result.data); // Mettre à jour les données des espaces
        } catch (error) {
            console.error("Erreur lors de la récupération des données :", error);
        }
    });

    // Calculer le nombre d'espaces
    let numberOfSpaces = derived(
        spaces,
        ($spaces) => $spaces.length,
        0 // Valeur par défaut
    );

    // Calculer la somme des tailles des espaces non disponibles
    const calculateTotalSize = $spaces => {
        let totalSize = 0;
        $spaces.forEach(space => {
            if (space.availibility == 2) { // Inclure seulement les espaces non disponibles
                totalSize += space.size; // Suppose que chaque espace a une propriété `size`
            }
        });
        return totalSize;
    };
    const calculateAvailableSpaces = $spaces => {
        let availableSpaces = 0;
        $spaces.forEach(space => {
            if (space.availibility == 1) {
                availableSpaces++;
            }
        });
        return availableSpaces;
    };

    // Store pour les statistiques
    let stats = derived(
        spaces,
        ($spaces) => [
            {
                text: "Total de coworkers :",
                value: calculateTotalSize($spaces).toString()
            },
            {
                text: "Espaces de coworking :",
                value: $spaces.length.toString()
            },
            {
                text: "Actuellement disponibles :",
                value: calculateAvailableSpaces($spaces).toString()
            }
        ]
    );
</script>

   




<nav class="navbar">
    {#each $stats as stat}
        <div>
            <h4>{stat.text}</h4>
            <strong
                >{ stat.value}</strong
            >
        </div>
    {/each}
</nav>

<style>
    nav {
        display: flex;
        justify-content: space-evenly;
        margin-top: 50px;
    }

    div {
        width: 220px;
        height: 70px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        text-align: start;
        background-color: rgb(204, 233, 242);
        padding: 5px 10px;
        border-radius: 2%;
        box-shadow: 4px 4px 8px rgb(92, 143, 158);
    }
    h4 {
        font-style: italic;
        font-weight: lighter;
    }
    strong {
        font-size: x-large;
    }
    @media screen and (max-width: 530px) {
        nav {
            margin-top: 25px;
        }
        div {
            width: 120px;
        }
        h4 {
            font-size: 0.8rem;
        }
        strong {
            font-size: medium;
        }
    }
</style>
