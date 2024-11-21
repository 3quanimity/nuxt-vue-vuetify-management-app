<template>
  <v-app>
      <v-layout>
          <v-app-bar class="ps-4 bg-indigo-darken-1" flat>
              <v-app-bar-title>Application de gestion</v-app-bar-title>
          </v-app-bar>
          <v-main>
              <div class="pa-4">
                  <slot />
              </div>
          </v-main>
      </v-layout>
  </v-app>
</template>
