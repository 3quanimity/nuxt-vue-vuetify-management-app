<template>
  <v-sheet class="mx-auto pa-4" width="100%">
    <h1 class="text-h4 mb-4">Projets</h1>

    <!-- Projects List -->
    <v-row>
      <template v-for="project in projectsWithImages">
        <ProjectSingle :project="project"></ProjectSingle>
      </template>
    </v-row>
  </v-sheet>
</template>

<script lang="ts" setup>
interface Project {
  id: string;
}

const { data, status, error, refresh } = await useFetch<Project[]>(
  "http://localhost:3002/projects"
);

const getRandomImage = () => {
  return "https://random.imagecdn.app/300/240";
};

const projectsWithImages = computed(() => {
  if (!data.value) return [];

  return data.value.map((project) => ({
    ...project,
    image: getRandomImage(),
  }));
});
</script>
