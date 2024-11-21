<template>
  <v-sheet class="mx-auto pa-4" width="100%">
    <h1 class="text-h4 mb-4">L'équipe</h1>

    <v-skeleton-loader v-if="loading" type="table" />

    <v-data-table
      v-else
      :headers="headers"
      :items="usersData"
      :items-per-page="10"
      hover
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Gestion des Utilisateurs</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>

          <!-- add new user dialog -->
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ props }">
              <v-btn class="mb-2" color="primary" dark v-bind="props">
                Ajouter
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">Ajouter un Utilisateur</span>
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

          <!-- delete user dialog -->
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5"
                >Supprimer un Utilisateur</v-card-title
              >

              <v-card-subtitle class="text-h7"
                >Vous etes sur le point de supprimer
                {{ idToDelete }} !</v-card-subtitle
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue-darken-1"
                  variant="text"
                  @click="closeDeleteDial"
                >
                  Annuler
                </v-btn>
                <v-btn
                  color="red-darken-1"
                  variant="text"
                  @click="deleteItemConfirm"
                >
                  Supprimer
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>

      <!-- isAdmin column -->
      <template v-slot:item.isAdmin="{ value }">
        <v-chip :color="value ? 'success' : 'error'" size="small">
          {{ value ? "Admin" : "User" }}
        </v-chip>
      </template>

      <!-- actions columns -->
      <template v-slot:item.actions="{ item }">
        <v-icon size="small" @click="deleteUser(item.id)">mdi-delete</v-icon>
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
const dialogDelete = ref(false);
const idToDelete = ref(-1);
const userData = ref({
  email: "",
  pass: "",
});

const headers = [
  { title: "ID", key: "id" },
  { title: "Name", key: "name" },
  { title: "Email", key: "email" },
  { title: "Status", key: "isAdmin" },
  { title: "Actions", key: "actions", sortable: false },
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

const deleteUser = (id) => {
  idToDelete.value = id;
  dialogDelete.value = true;
};

const deleteItemConfirm = async () => {
  const { data: responseData } = await useFetch(
    `http://localhost:3002/users?id=${idToDelete.value}`,
    { method: "delete" }
  );

  if (responseData.value) {
    closeDeleteDial();
    fetchUsers();
  }
};

const closeDeleteDial = () => {
  dialogDelete.value = false;
  idToDelete.value = -1;
};

// LIFE CYCLES
// TODO: fix list not loading on refresh
onMounted(() => {
  fetchUsers();
});
</script>
