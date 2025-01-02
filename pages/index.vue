<template>
  <v-row justify="center" align="center">
    <div class="title-container">
      <h1>Gestor de Tareas</h1>
      <AddTaskButton />
    </div>
    <v-col cols="12" sm="12" md="12">
      <ListTask />
    </v-col>
  </v-row>
</template>

<script>
import AddTaskButton from "@/components/add-task-button.vue";
import ListTask from "@/components/list-task.vue";

export default {
  name: 'IndexPage',
  components: {
    AddTaskButton,
    ListTask
  },
}
</script>

<style>
.title-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 16px;
  flex: 1;
}
</style>