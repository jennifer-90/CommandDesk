<template>
  <span>{{ produit.description }} - {{ produit.PU.toFixed(2) }} €</span>
  <span v-if="produit.ageMin >= 16"> &#128721;Avoir au moins 16 ans &#128721;</span>
</template>

<script setup>
defineProps(["produit"]);
</script>

<style scoped></style>
