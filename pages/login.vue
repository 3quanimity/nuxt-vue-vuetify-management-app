<template>
  <v-sheet class="mx-auto" width="100%">
      <h1>Se connecter</h1>
      <v-spacer/>
      <v-alert
          v-show="error"
          text="Olala, ça n'a pas marché ! Soit le mail n'est pas bon, soit le mot de passe n'est pas bon (ou les deux je sais pas)."
          title="Erreur de connexion"
          type="error"
      ></v-alert>
 
      <v-text-field class="mt-4" label="email" v-model="userData.email"></v-text-field>
      <v-text-field label="pass" type="password" v-model="userData.pass"></v-text-field>
      <v-btn @click="login">
          Connexion
      </v-btn>
  </v-sheet>
</template>

<script setup lang="ts">
import { ref } from 'vue'

definePageMeta({
  layout: 'login'
})

const userData = ref({
  email: '',
  pass: '',
})

const error = ref(false)

const login = async () => {
  const { data: responseData } = await useFetch(`http://localhost:3002/checkUser?email=${userData.value.email}&pass=${userData.value.pass}`, {
      method: 'get',
  })

  if (responseData.value.length == 0) {
      error.value = true
  } else {
      localStorage.setItem("isAdmin", "true");
      navigateTo("/");
  }
}

</script>