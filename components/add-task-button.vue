<template>
    <v-dialog v-model="dialog" persistent max-width="600px">
        <template #activator="{ on, attrs }">
            <v-btn color="primary" dark rounded elevation="2" v-bind="attrs" v-on="on">
                Crear tarea
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                <span class="text-h5">Agregar una tarea</span>
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
                            <v-combobox v-model="form.tags" hide-selected hint="Maximo de 5 tags" label="Agregar tags" multiple persistent-hint small-chips />
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
                <v-btn color="primary" :disabled="!isFormValid" @click="addTask">
                    Crear tarea
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import constants from './../assets/constants';

export default {
    name: "AddTaskButton",
    data() {
        return {
            dialog: false,
            form: {
                title: '',
                status: null,
                date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().slice(0, 10),
                comments: '',
                tags: [],
                description: '',
            },
            status: ['No Completada', 'Completada'],
        };
    },
    computed: {
        isFormValid() {
            return this.form.title && this.form.status;
        },
    },
    methods: {
        async addTask() {
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
                const response = await fetch(constants.endpoint, {method: 'POST', headers, body: formData,});

                await response.json();
                this.dialog = false;
            } catch (error) {
                this.dialog = false;
                alert(`Error al crear la tarea: ${error}`);
            }
        },
    },
};
</script>
