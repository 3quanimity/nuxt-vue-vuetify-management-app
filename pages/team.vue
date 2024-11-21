<template>
    <v-sheet class="mx-auto pa-4" width="100%">
        <h1 class="text-h4 mb-4">L'équipe</h1>
        
        <v-skeleton-loader 
            v-if="loading" 
            type="table"
        />
        
        <v-data-table 
            v-else
            :headers="headers"
            :items="usersData"
            :items-per-page="10"
            density="comfortable"
            hover
            class="elevation-1"
        >
            <template v-slot:item.isAdmin="{ value }">
                <v-chip 
                    :color="value ? 'success' : 'error'" 
                    size="small"
                >
                    {{ value ? 'Admin' : 'User' }}
                </v-chip>
            </template>

            <template v-slot:no-data>
                <v-alert 
                    type="info" 
                    title="No Users" 
                    text="No user data available"
                />
            </template>
        </v-data-table>
    </v-sheet>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface User {
    id: string
    name: string
    email: string
    isAdmin: boolean
}

const usersData = ref<User[]>([])
const loading = ref(true)

const headers = [
    { title: 'ID', key: 'id' },
    { title: 'Name', key: 'name' },
    { title: 'Email', key: 'email' },
    { title: 'Status', key: 'isAdmin' }
]

const fetchUsers = async () => {
    try {
        const { data } = await useFetch<User[]>("http://localhost:3002/users")
        if (data.value) {
            usersData.value = data.value
            usersData.value = usersData.value.map(user => ({
                name: getNameFromEmail(user.email),
                ...user
            }))
        }
    } catch (error) {
        console.error('Failed to fetch users:', error)
    } finally {
        loading.value = false
    }
}

const getNameFromEmail = (email) => {
    return email.split("@")[0]
}

onMounted(fetchUsers)
</script>