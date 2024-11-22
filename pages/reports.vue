<template>
  <h1 class="mb-4">Rapport Détaillé</h1>

  <v-row>
    <v-col>
      <v-card class="mx-auto text-center" color="indigo" max-width="600" dark>
        <v-card-text>
          <v-sheet color="rgba(0, 0, 0, .12)">
            <v-sparkline
              :model-value="salesValues"
              color="rgba(255, 255, 255, .7)"
              height="100"
              padding="24"
              stroke-linecap="round"
              smooth
            >
              <template v-slot:label="item"> ${{ item.value }} </template>
            </v-sparkline>
          </v-sheet>
        </v-card-text>

        <v-card-text>
          <div class="text-h4 font-weight-thin">Progression des ventes</div>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions class="justify-center">
          <v-btn variant="text" block> Plus d'infos </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>

    <v-col>
      <v-card class="mx-auto text-center" color="indigo" max-width="600" dark>
        <v-card-text>
          <v-sheet color="rgba(0, 0, 0, .12)">
            <v-sparkline
              :model-value="usersValues"
              color="rgba(255, 255, 255, .7)"
              height="100"
              padding="24"
              stroke-linecap="round"
              smooth
            >
              <template v-slot:label="item">
                {{ item.value }}
              </template>
            </v-sparkline>
          </v-sheet>
        </v-card-text>

        <v-card-text>
          <div class="text-h4 font-weight-thin">Progression des clients</div>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions class="justify-center">
          <v-btn variant="text" block> Plus d'infos </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>

  <h2 class="text-center my-8">Principaux KPI</h2>

  <v-row>
    <v-col>
      <div class="text-center">
        <v-progress-circular
          model-value="60"
          :rotate="360"
          :size="100"
          :width="15"
          color="indigo"
        >
          <template v-slot:default> 60 % </template>
        </v-progress-circular>
        <p>Délais</p>
      </div>
    </v-col>
    <v-col>
      <div class="text-center">
        <v-progress-circular
          model-value="80"
          :rotate="360"
          :size="100"
          :width="15"
          color="indigo"
        >
          <template v-slot:default> 80 % </template>
        </v-progress-circular>
        <p>Satisfaction</p>
      </div>
    </v-col>
    <v-col>
      <div class="text-center">
        <v-progress-circular
          model-value="70"
          :rotate="360"
          :size="100"
          :width="15"
          color="indigo"
        >
          <template v-slot:default> 70 % </template>
        </v-progress-circular>
        <p>Efficacité</p>
      </div>
    </v-col>
    <v-col>
      <div class="text-center">
        <v-progress-circular
          model-value="35"
          :rotate="360"
          :size="100"
          :width="15"
          color="indigo"
        >
          <template v-slot:default> 35 % </template>
        </v-progress-circular>
        <p>Marge</p>
      </div>
    </v-col>
  </v-row>

  <h2 class="text-center my-8">Derniers commentaires</h2>

  <v-timeline>
    <v-timeline-item v-for="i in 5">
      <template v-slot:icon>
        <v-avatar :image="`https://robohash.org/${i}`"></v-avatar>
      </template>
      <template v-slot:opposite>
        <span>Impressionant</span>
      </template>
      <v-card class="elevation-2">
        <v-card-title class="text-h5"> Lorem ipsum </v-card-title>
        <v-card-text
          >Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore
          culpa architecto maiores nihil temporibus numquam. Saepe laudantium ea
          labore unde nihil aut nam sint numquam earum, perferendis, aliquam,
          deserunt quam.</v-card-text
        >
      </v-card>
    </v-timeline-item>
  </v-timeline>
</template>

<script lang="ts" setup>
const salesValues = [423, 446, 675, 510, 590, 610, 760];

const usersValues = [4, 9, 45, 89, 135, 211, 289];
</script>
