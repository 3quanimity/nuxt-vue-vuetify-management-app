<template>
    <v-sheet class="mx-auto" width="100%">
        <h1>L'équipe</h1>
        <v-spacer/>
        <v-data-table :items="usersData" v-if="usersData"></v-data-table>
    </v-sheet>
</template>

<script lang="ts" setup>
  const { data : usersData } = await useFetch("http://localhost:3002/users");
</script>