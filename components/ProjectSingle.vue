<template>
  <v-col cols="12" md="4">
    <v-card class="mx-auto" color="grey-lighten-4" variant="flat">
      <v-img
        class="align-end text-white"
        height="200"
        :src="project.image"
        cover
      >
        <v-card-title>{{ project.name }}</v-card-title>
      </v-img>

      <v-card-subtitle class="pt-4"> 10k€ </v-card-subtitle>

      <v-card-text>
        <div>{{ project.desc }}</div>
      </v-card-text>

      <v-card-actions>
        <v-btn
          color="indigo"
          text="Voir"
          @click="goToProject(project.id)"
        ></v-btn>
        <v-btn
          color="indigo"
          text="Supprimer"
          @click="deleteProject(project.id)"
        ></v-btn>
      </v-card-actions>
    </v-card>
  </v-col>
</template>

<script lang="ts" setup>
type ProjectProps = {
  project: Object;
};

const props = defineProps<ProjectProps>();

const emit = defineEmits(["deleteProject"]);

const goToProject = async (id: any) => {
  console.log(id);

  await navigateTo({ name: "project-id", params: { id: id } });
};

const deleteProject = (id: any) => {
  emit("deleteProject", id);
};
</script>
