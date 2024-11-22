<template>
  <v-sheet class="mx-auto pa-4" width="100%">
    <h1 class="text-h4 mb-4">Projets</h1>

    <!-- Projects List -->
    <v-row>
      <template v-for="project in projectsWithImages">
        <ProjectSingle
          :project="project"
          @delete-project="deleteProject"
        ></ProjectSingle>
      </template>
    </v-row>

    <v-divider class="mt-8"></v-divider>

    <!-- Crete new project  -->
    <v-expansion-panels>
      <v-expansion-panel>
        <v-expansion-panel-title class="text-h4"
          >Ajouter un projet</v-expansion-panel-title
        >
        <v-expansion-panel-text>
          <form @submit.prevent="submit">
            <v-text-field
              v-model="name.value.value"
              :counter="3"
              :error-messages="name.errorMessage.value"
              label="Nom"
            ></v-text-field>

            <v-text-field
              v-model="desc.value.value"
              :error-messages="desc.errorMessage.value"
              label="Description"
            ></v-text-field>

            <v-text-field
              v-model="user.value.value"
              :counter="1"
              :error-messages="user.errorMessage.value"
              label="Utilisateur"
            ></v-text-field>

            <v-btn class="me-4" type="submit"> Valider </v-btn>

            <v-btn @click="handleReset"> Réinitialiser </v-btn>
          </form>
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-sheet>
</template>

<script lang="ts" setup>
import { useField, useForm } from "vee-validate";

interface Project {
  id: string;
}

const projectsWithImages = computed(() => {
  if (!data.value) return [];

  return data.value.map((project) => ({
    ...project,
    image: getRandomImage(),
  }));
});

const { data, status, error, refresh } = await useFetch<Project[]>(
  "http://localhost:3002/projects"
);

const getRandomImage = () => {
  return "https://random.imagecdn.app/300/240";
};

const { handleSubmit, handleReset } = useForm({
  validationSchema: {
    name(value: any) {
      if (value?.length >= 3) return true;
      return "Le nom doit contenir au moins 3 caractères";
    },
    desc(value: any) {
      if (value?.length >= 3) return true;
      return "Le nom doit contenir au moins 3 caractèes";
    },
    user(value: any) {
      if (/^[0-9-]{1,}$/.test(value)) return true;
      return "Il faut au moins 1 chiffre";
    },
  },
});

const name = useField("name");
const desc = useField("desc");
const user = useField("user");

const submit = handleSubmit((values) => {
  //   alert(JSON.stringify(values, null, 2));
  const projectToCreate = JSON.stringify(values, null, 2);
  createProject(projectToCreate);
});

const createProject = async (project: any) => {
  const { data: responseData } = await useFetch(
    `http://localhost:3002/projects`,
    {
      method: "post",
      body: project,
    }
  );

  if (responseData.value) {
    handleReset();
    refresh();
  }
};

const deleteProject = async (idToDelete: any) => {
  const { data: responseData } = await useFetch(
    `http://localhost:3002/projects?id=${idToDelete}`,
    { method: "delete" }
  );

  if (responseData.value) {
    refresh();
  }
};
</script>
