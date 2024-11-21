<template>
    <v-app>
        <v-layout>
            <!-- NAVIGATION DRAWER  -->
            <v-navigation-drawer v-model="drawer"
            class="bg-indigo"
            theme="dark"
            permanent
            >
            <v-list density="compact" item-props nav>
                <v-list-subheader>ADMINISTRATION</v-list-subheader>
                <v-list-item 
                v-for="item in items"
                :key="item.title"
                :title="item.title"
                :prepend-icon="item.prependIcon"
                @click="navigateToPage(item.url)"
                />
            </v-list>
            </v-navigation-drawer>

            <!-- APP BAR  -->
            <v-app-bar class="ps-4 bg-indigo-darken-1" flat>
                <v-app-bar-nav-icon v-if="$vuetify.display.smAndDown" @click="drawer = !drawer" />
                <v-app-bar-title>Application de Gestion</v-app-bar-title>
                <template #append>
                    <v-btn class="text-none me-2" height="48" icon slim>
                        <v-avatar color="surface-light" image="https://robohash.org/vuejs" size="32" />

                        <v-menu activator="parent">
                            <v-list density="compact" nav>
                                <v-list-item append-icon="mdi-cog-outline" link title="Options" @click="navigateToPage('/settings')"/>
                                <v-list-item append-icon="mdi-logout" link title="Se déconnecter" @click="logOut" />
                            </v-list>
                        </v-menu>
                    </v-btn>
                </template>
            </v-app-bar>

            <v-main>
                <div class="pa-4" v-if="showContent">
                    <slot/>
                </div>
            </v-main>
        </v-layout>
    </v-app>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'

// vars
const drawer = ref(true)
const items = ref([
    {
        title: 'Dashboard',
        prependIcon: 'mdi-view-dashboard-outline',
        link: true,
        url: "/"
    },
    {
        title: 'Team',
        prependIcon: 'mdi-account-group',
        link: true,
        url: "/team"
    },
    {
        title: 'Projects',
        prependIcon: 'mdi-briefcase-outline',
        link: true,
        url: "/projects"
    },
    {
        title: 'Reports',
        prependIcon: 'mdi-file-chart-outline',
        link: true,
        url: "/reports"
    },
])
const showContent = ref(false)

// lifecycles
onMounted(() => {
    const isAdminStorage = localStorage.getItem("isAdmin")
    if(isAdminStorage == "true") {
        showContent.value = true
    } else {
        navigateTo('/login')
    }
})

// methods
const navigateToPage = async (page: any) => {
    await navigateTo(page)
}
</script>
