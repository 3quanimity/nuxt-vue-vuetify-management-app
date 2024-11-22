<template>
  <div>
    <h1>Détail du Projet</h1>
    {{ route.params }}
  </div>
</template>

<script lang="ts" setup>
const route = useRoute();
</script>
