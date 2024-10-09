<template>
  <div>
    <h2>Votre commande :</h2>
    <div v-if="tableauDesProduits.length > 0">
      <ul>
        <li v-for="produit in tableauDesProduits" :key="produit.code">
          {{ produit.description }} - [{{ produit.PU.toFixed(2) }} €] ({{ produit.quantity }})
          <button class="croixRouge" @click="supprimerProduit(produit)">&#10060;</button>
          <button class="boutonModifierQuantite" @click="incrementerQuantite(produit)"> &#10133;</button>
          <button class="boutonModifierQuantite" @click="decrementerQuantite(produit)"> &#10134;</button>
        </li>
      </ul>
      <button class="boutonPayer" @click="effectuerPaiement">
        &#129530; PAYER: {{ prixTotal.toFixed(2) }} €
      </button>
    </div>
    <div v-else>
      *** Vous n'avez encore rien ajouté ***
    </div>
  </div>
</template>


<script setup>
defineProps({
  tableauDesProduits: {
    type: Array,
    required: true
  },
  prixTotal: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['produitSupprime', 'produitQuantiteIncrementee', 'produitQuantiteDiminuee', 'paiementEffectue']);

function supprimerProduit(produit) { // fct 2 de "app"
  emit('produitSupprime', produit);
}

function incrementerQuantite(produit) { // fct 3 de "app"
  emit('produitQuantiteIncrementee', produit);
}

function decrementerQuantite(produit) { //fct 4 de "app"
  emit('produitQuantiteDiminuee', produit);
}

function effectuerPaiement() { // fct 5 de "app"
  emit('paiementEffectue');
}
</script>

<style scoped>
.croixRouge {
  background-color: transparent;
  border: none;
  font-size: 12px;
  padding: 2px 4px;
  cursor: pointer;
  color: red;
  border-radius: 3px;
}

.boutonModifierQuantite {
  background-color: transparent;
  color: white;
  border: none;
  padding: 2px 2px;
  margin: 0.2rem;
  border-radius: 3px;
}

.boutonPayer {
  position: absolute;
  top: 20px;
  right: 20px;
  background-color: lightgreen;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  color: black;
  font-weight: bold;
}
</style>