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
                    <v-list-item-action>
                        <SeeTask :task-id="item.id" @task-updated="updateTask" />
                    </v-list-item-action>
                    <v-list-item-action>
                        <RemoveTask :task-id="item.id" @task-removed="removeTask" />
                    </v-list-item-action>
                </v-list-item>
            </v-list-item-group>
        </v-list>
    </v-card>
</template>

<script>
import constants from './../assets/constants';
import SeeTask from './see-task.vue';
import RemoveTask from './remove-task.vue';

export default {
    name: "ListTask",
    components: {
        SeeTask,
        RemoveTask
    },
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

                this.tasks = data;
                this.loading = false;
            } catch (error) {
                alert(`Error al obtener las tareas: ${error}`);
            }
        },
        removeTask(taskId) {
            const index = this.tasks.findIndex(task => task.id === taskId);
            if (index !== -1) {
                this.tasks.splice(index, 1);
            }
        },
        updateTask(updatedTask) {
            const index = this.tasks.findIndex(task => task.id === updatedTask.id);
            if (index !== -1) {
                this.$set(this.tasks, index, updatedTask);
            }
        },
    },
}
</script>
