<template>
  <div>
    <h1>&#127861; CAFETERIA &#127789;</h1>
    <hr>

    <!----- *** - COMMANDE - ***  ----->
    <ListeCommande
        :tableauDesProduits="TableauDesProduitsSelectionnes"
        :prixTotal="prixTotal"
        @produitSupprime="supprimerProduit"
        @produitQuantiteIncrementee="incrementerQuantite"
        @produitQuantiteDiminuee="decrementerQuantite"
        @paiementEffectue="effectuerPaiement"
    />

    <!----- *** - LA CARTE - ***  ---->
    <div class="menu-container">
      <div class="menu-section">
        <h3>BOISSONS</h3>
        <tarif
            :produits="boissons"
            @produitChoisi="ajoutDansTableauDesProduitsSelectionnes"/>
      </div>

      <div class="menu-section">
        <h3>SNACKS</h3>
        <tarif
            :produits="snacks"
            @produitChoisi="ajoutDansTableauDesProduitsSelectionnes"/>
      </div>
    </div>

  </div>
</template>


<script setup>
/*---------IMPORT---------*/
import {computed, onMounted, ref} from "vue";
import tarif from "./components/tarif.vue";
import ListeCommande from "./components/ListeCommande.vue";


/*--------VARIABLES REACTIVES-------*/
const produits = ref([]);
const TableauDesProduitsSelectionnes = ref([]); //Tableau des produits choisis

const boissons = computed(() => produits.value.filter(x => x.categorie == "boisson"));
const snacks = computed(() => produits.value.filter(x => x.categorie == "snack"));

const prixTotal = computed(() => {
  let total = 0;
  for (let i = 0; i < TableauDesProduitsSelectionnes.value.length; i++) {
    let produit = TableauDesProduitsSelectionnes.value[i];

    total += produit.PU * produit.quantity;
  }
  return total;
})


/*-------MOUNTED-------*/
onMounted(() => {
  fetch("tarif.json")
      .then(rep => rep.json())
      .then(rep => {
        produits.value = rep.produits;
      });
});

/*-------METHODS -------*/

function ajoutDansTableauDesProduitsSelectionnes(produit) {
  const produitExistant = TableauDesProduitsSelectionnes.value.find(p => p.code === produit.code);

  if (produitExistant) {
    produitExistant.quantity++;
  } else {
    TableauDesProduitsSelectionnes.value.push({...produit, quantity: 1});// Ajout du produit avec une quantité de 1
  }
}

function supprimerProduit(produit) {
  //croix rouge
  TableauDesProduitsSelectionnes.value = TableauDesProduitsSelectionnes.value.filter(p => p.code !== produit.code);
}

function incrementerQuantite(produit) {
  const produitExistant = TableauDesProduitsSelectionnes.value.find(p => p.code === produit.code);

  if (produitExistant) {
    produitExistant.quantity++;
  }
}

function decrementerQuantite(produit) {
  const existant = TableauDesProduitsSelectionnes.value.find(p => p.code === produit.code);

  if (existant && existant.quantity > 1) {
    existant.quantity--;
  } else {
    supprimerProduit(produit);
  }
}

function effectuerPaiement() {

  let resume = " 🥳 RESUMER DE VOTRE COMMANDE: \n\n ";

  TableauDesProduitsSelectionnes.value.forEach(produit => {
    resume += `🔸${produit.description} - ${produit.quantity} x ${produit.PU.toFixed(2)} € = ${(produit.quantity *
        produit.PU).toFixed(2)} €\n`;
  });
  resume += `\n🧺 TOTAL : ${prixTotal.value.toFixed(2)} €`;

  alert(resume);
  TableauDesProduitsSelectionnes.value = [];
}
</script>


<style scoped>

.menu-container {
  display: flex;
  justify-content: space-between;
  gap: 2rem;
}

.menu-section {
  flex: 1;
  padding: 1rem;
  border: 1px dashed rgba(255, 140, 0, 0.37);
}

h1, h3 {
  text-align: center;
}
</style>
