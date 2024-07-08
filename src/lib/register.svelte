<script>

let nom = "";
  let prenom = "";
  let email = "";
  let password = "";
  let success = false;
  let error = false;
  let showPasswordCriteria = false;

  function aDesCaractèresSpéciaux(valeurAVerifier) {
    const lesCaractereSpeciaux = /[`!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?~]/;
    return lesCaractereSpeciaux.test(valeurAVerifier);
  }

  function estUneAdresseEmail(valeurAVerifier) {
    const formatEmail = /^[\w\.-]+@[\w\.-]+\.[a-zA-Z]{2,}$/;
    return formatEmail.test(valeurAVerifier);
  }

  function estUnMotDePasse(valeurAVerifier) {
    if (valeurAVerifier.length < 8 || valeurAVerifier.length > 16) {
      return false;
    }
    const maj = valeurAVerifier.match(/[A-Z]/g);
    if (maj === null || maj.length < 1) {
      return false;
    }
    const min = valeurAVerifier.match(/[a-z]/g);
    if (min === null || min.length < 1) {
      return false;
    }
    const num = valeurAVerifier.match(/[0-9]/g);
    if (num === null || num.length < 1) {
      return false;
    }
    return true;
  }

  function handleSubmit(event) {
    event.preventDefault();

    if (
      !aDesCaractèresSpéciaux(nom) &&
      !aDesCaractèresSpéciaux(prenom) &&
      estUneAdresseEmail(email) &&
      estUnMotDePasse(password)
    ) {
      success = true;
      error = false;
    } else {
      success = false;
      error = true;
    }
  }


</script>


    <h1>S'enregister</h1>

    <div class=" alert success" class:show={success}>
        <p> Inscription réussie</p>

    </div>


    <div class=" alert error" class:show={error}>
        <p> Erreur lors de l'inscription</p>

    </div>

    <form class="register" aria-label="informations register"  on:submit={handleSubmit}>
        <div>
            <label for="name">Nom</label>
            <label for="firstname">Prénom</label>

            <input required type="text" placeholder="ex Dupont" name="name" id="name"    bind:value={nom}/>

            <input required type="text" placeholder="ex Michel" name="firstname" id="firstname"   bind:value={prenom} />



        </div>

        <label for="email">Email</label>
        <input required type="email" id="email" name="email" placeholder="ex : m.dupont@monmail.com"    bind:value={email}/>

        <label for="password">Mot de passe</label>
        <input required type="password" id="password" name="password" placeholder="******"    bind:value={password} 
          
        on:mouseover={() => showPasswordCriteria = true}
        on:mouseout={() => showPasswordCriteria = false} 
        on:focus={() => showPasswordCriteria = true}
        on:blur={() => showPasswordCriteria = false}/>




        <div class="password-criteria" class:show={showPasswordCriteria}>
            <p>Votre mot de passe doit :</p><br>
            <ul>
              <li>* Être composé de 8 à 16 caractères</li>
              <li>* Contenir au moins une majuscule</li>
              <li>* Contenir au moins une minuscule</li>
              <li>* Contenir au moins un chiffre</li>
            </ul>
          </div>

        <input type="submit" class="button" value="Créer un compte" />



    </form>




