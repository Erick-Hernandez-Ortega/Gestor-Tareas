<template>
    <v-card class="mx-auto" dark :loading="loading" shaped>
        <v-list color="primary">
            <v-subheader>Tareas</v-subheader>
            <v-list-item-group>
                <v-list-item v-for="(item, i) in tasks" :key="i">
                    <v-list-item-icon>
                        <v-icon v-if="item.is_completed === 1" color="green">mdi-checkbox-marked-circle</v-icon>
                        <v-icon v-else color="yellow">mdi-timer-sand-complete</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list-item-group>
        </v-list>
    </v-card>
</template>

<script>
import constants from './../assets/constants';

export default {
    name: "ListTask",
    data() {
        return {
            tasks: [],
            loading: true,
        }
    },
    created() {
        this.fetchTasks();
    },
    methods: {
        async fetchTasks() {
            try {
                const response = await fetch(`${constants.endpoint}?token=${constants.token}`, {
                    headers: {
                        'Authorization': `Bearer ${constants.token}`,
                        'Content-Type': 'application/json',
                    },
                });
                const data = await response.json();
                console.log(data, 'data');
                this.tasks = data;
                this.loading = false;
            } catch (error) {
                alert(error);
            }
        },
    },
}
</script>
