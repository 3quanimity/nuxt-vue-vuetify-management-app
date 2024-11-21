<template>
    <h1>Accueil</h1>
  <v-card
    class="mx-auto"
    max-width="368"
    color="indigo"
  >
    <v-card-item title="Utilisateurs">
      <template v-slot:subtitle>
        <v-icon
          class="me-1 pb-1"
          color="success"
          icon="mdi-trending-up"
          size="18"
        ></v-icon>

        Extreme Weather Alert
      </template>
    </v-card-item>

    <v-card-text class="py-0">
      <v-row align="center" no-gutters>
        <v-col
          class="text-h2"
          cols="6"
        >
          42
        </v-col>

        <v-col class="text-right" cols="6">
          <v-icon
            icon="mdi-account"
            size="88"
          ></v-icon>
        </v-col>
      </v-row>
    </v-card-text>

    <div class="d-flex py-3 justify-space-between">
      <v-list-item
        density="compact"
        prepend-icon="mdi-weather-windy"
      >
        <v-list-item-subtitle>123 km/h</v-list-item-subtitle>
      </v-list-item>

      <v-list-item
        density="compact"
        prepend-icon="mdi-weather-pouring"
      >
        <v-list-item-subtitle>48%</v-list-item-subtitle>
      </v-list-item>
    </div>

    <v-divider></v-divider>

    <v-card-actions>
      <v-btn>TODO</v-btn>
    </v-card-actions>
  </v-card>

</template>

<script lang="ts" setup>
    const { data } = await useFetch('http://localhost:3002/users')
</script>
