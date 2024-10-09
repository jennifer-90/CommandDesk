<template>
  <div>
    <ul>
      <li v-for="p in produits" :key="p.code">
        <button
            :value="p"
            @click="ajoutDansTableauDesProduitsSelectionnes(p)">
          <lg-tarif :produit="p"/>
        </button>
      </li>
    </ul>
  </div>
</template>


<script setup>
/*---------IMPORT---------*/
import LgTarif from "./lgTarif.vue";
import {defineProps, defineEmits} from "vue";


/*--------VARIABLES-------*/
defineProps(["produits"]);
const emit = defineEmits(["produitChoisi"]);


/*---------FONCTIONS-------*/
function ajoutDansTableauDesProduitsSelectionnes(produit) { //fct 1 de "app"
  emit("produitChoisi", produit);
}

</script>


<style scoped>

button {
  width: 25rem;
}

li {
  list-style-type: none;

}
</style>


