<template>
    <v-dialog v-model="dialog" persistent max-width="600px">
        <template #activator="{ on, attrs }">
            <v-icon color="red" v-bind="attrs" v-on="on">mdi-delete-circle</v-icon>
        </template>

        <v-card :loading="loading">
            <v-card-title>
                <span class="text-h5">¿Desea borrar la tarea?</span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-row>
                        <p>Este proceso es irreversible, se perderan todos los datos de la tarea</p>
                    </v-row>
                </v-container>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" text @click="dialog = false">
                    Cerrar
                </v-btn>
                <v-btn color="red" @click="removeTask">
                    Borrar tarea
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import constants from './../assets/constants';

export default {
    name: "RemoveTask",
    props: {
        taskId: {
            type: [Number],
            required: true,
        },
    },
    data() {
        return {
            dialog: false,
            loading: false,
        };
    },
    methods: {
        removeTask() {
            this.loading = true;
            try {
                fetch(`${constants.endpoint}/${this.taskId}?token=${constants.token}`, {
                    method: 'DELETE',
                    headers: {
                        'Authorization': `Bearer ${constants.token}`,
                        'Content-Type': 'application/json',
                    },
                }).then(() => {
                    this.closeDialiog();
                })
            } catch (error) {
                alert(`Error al borrar la tarea: ${error}`);
                this.closeDialiog();
            }
        },
        closeDialiog() {
            this.dialog = false;
            this.loading = false;
        }
    }
}
</script>