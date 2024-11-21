<template>
  <v-sheet class="mx-auto pa-4" width="100%">
    <h1 class="text-h4 mb-4">L'équipe</h1>

    <v-skeleton-loader v-if="loading" type="table" />

    <v-data-table
      v-else
      :headers="headers"
      :items="usersData"
      :items-per-page="10"
      density="comfortable"
      hover
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Gestion des Utilisateurs</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ props }">
              <v-btn class="mb-2" color="primary" dark v-bind="props">
                Ajouter
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">Utilisateur</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-text-field
                    label="Email"
                    v-model="userData.email"
                  ></v-text-field>
                  <v-text-field
                    label="Mot de passe"
                    v-model="userData.pass"
                  ></v-text-field>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" variant="text" @click="closeDial">
                  Annuler
                </v-btn>
                <v-btn color="blue-darken-1" variant="text" @click="addUser">
                  Ajouter
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>

      <template v-slot:item.isAdmin="{ value }">
        <v-chip :color="value ? 'success' : 'error'" size="small">
          {{ value ? "Admin" : "User" }}
        </v-chip>
      </template>
    </v-data-table>
  </v-sheet>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

interface User {
  id: string;
  name: string;
  email: string;
  isAdmin: boolean;
}

// VARS
const usersData = ref<User[]>([]);
const loading = ref(true);
const dialog = ref(false);
const userData = ref({
  email: "",
  pass: "",
});

const headers = [
  { title: "ID", key: "id" },
  { title: "Name", key: "name" },
  { title: "Email", key: "email" },
  { title: "Status", key: "isAdmin" },
];

// METHODS
const fetchUsers = async () => {
  try {
    const { data } = await useFetch<User[]>("http://localhost:3002/users");
    if (data.value) {
      usersData.value = data.value;
      usersData.value = usersData.value.map((user) => ({
        name: getNameFromEmail(user.email),
        ...user,
      }));
    }
  } catch (error) {
    console.error("Failed to fetch users:", error);
  } finally {
    loading.value = false;
  }
};

const getNameFromEmail = (email) => {
  return email.split("@")[0];
};

const closeDial = () => {
  dialog.value = false;
};

const addUser = async () => {
  const newUser = {
    email: userData.value.email,
    pass: userData.value.pass,
  };

  const { data } = await useFetch("http://localhost:3002/users", {
    method: "post",
    body: newUser,
  });

  if (data.value) {
    userData.value.email = "";
    userData.value.pass = "";
    closeDial();
    fetchUsers();
  }
};

// LIFE CYCLES
// TODO: fix list not loading on refresh
onMounted(() => {
  fetchUsers();
});
</script>
