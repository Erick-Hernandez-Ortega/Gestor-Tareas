<template>
    <v-dialog v-model="dialog" persistent max-width="600px">
        <template #activator="{ on, attrs }">
            <v-icon v-bind="attrs" v-on="on" @click="openDialog">mdi-eye-circle</v-icon>
        </template>

        <v-card :loading="loading">
            <v-card-title>
                <span class="text-h5">Tarea</span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-row>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="form.title" label="Titulo*" required></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-select v-model="form.status" :items="status" label="Estatus*" required></v-select>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-menu :close-on-content-click="false" :nudge-right="40" transition="scale-transition" offset-y min-width="auto">
                                <template #activator="{ on, attrs }">
                                    <v-text-field v-model="form.date" label="Fecha" prepend-icon="mdi-calendar" readonly v-bind="attrs" v-on="on"></v-text-field>
                                </template>
                                <v-date-picker v-model="form.date" locale="es" @input="menu2 = false"></v-date-picker>
                            </v-menu>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="form.comments" label="Comentarios"></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-combobox v-model="form.tags" hide-selected hint="Presionar enter para agregar tag" label="Agregar tags" multiple persistent-hint small-chips />
                        </v-col>
                        <v-col cols="12">
                            <v-textarea v-model="form.description" label="Descripción" hint="Describe bien la tarea"></v-textarea>
                        </v-col>
                    </v-row>
                </v-container>
                <small>*Obligatorios</small>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" text @click="dialog = false">
                    Cerrar
                </v-btn>
                <v-btn color="primary" :disabled="!isFormValid" @click="editTask">
                    Editar tarea
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import constants from './../assets/constants';

export default {
    name: "SeeTask",
    props: {
        taskId: {
            type: [Number],
            required: true,
        },
    },
    data() {
        return {
            dialog: false,
            form: {
                title: null,
                status: null,
                date: null,
                comments: null,
                tags: [],
                description: null,
            },
            status: ['No Completada', 'Completada'],
            loading: true,
        };
    },
    computed: {
        isFormValid() {
            return this.form.title && this.form.status;
        },
    },
    methods: {
        openDialog() {
            this.dialog = true;
            this.fetchTask(this.taskId);
        },

        async fetchTask(id) {
            const response = await fetch(`${constants.endpoint}/${id}?token=${constants.token}`, {
                headers: {
                    'Authorization': `Bearer ${constants.token}`,
                    'Content-Type': 'application/json',
                },
            });

            try {
                const data = await response.json();

                this.form = {
                    ...data[0],
                    date: data[0].due_date,
                    status: data[0].is_completed ? 'Completada' : 'No Completada',
                    due_date: undefined,
                    tags: data[0].tags !== null && data[0].tags !== "" ? data[0].tags?.trim().split(',') : [],
                };

                this.loading = false;
            } catch (error) {
                this.dialog = false;
                alert(`Error al obtener la tarea: ${error}`);
            }
        },
        async editTask() {
            const formData = new URLSearchParams();
            formData.append('token', constants.token);
            formData.append('title', this.form.title);
            formData.append('is_completed', this.form.status === 'Completada' ? 1 : 0);
            formData.append('due_date', this.form.date);
            formData.append('comments', this.form.comments);
            formData.append('description', this.form.description);
            formData.append('tags', this.form.tags.join(','));

            const headers = {
                'Authorization': `Bearer ${constants.token}`,
                'Content-Type': 'application/x-www-form-urlencoded',
            };

            try {
                const response = await fetch(`${constants.endpoint}/${this.taskId}`, {
                    method: 'PUT',
                    headers,
                    body: formData,
                });

                await response.json();
                this.dialog = false;
            } catch (error) {
                this.dialog = false;
                alert(`Error al editar la tarea: ${error}`);
            }
        },
    },
}
</script>
