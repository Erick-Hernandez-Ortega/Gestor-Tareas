# Gestor de tareas

Este es un proyecto para gestionar tareas usando Vue.js, Vuetify y la API proporcionada. El objetivo es poder agregar, editar, eliminar y listar tareas a través de una interfaz de usuario utilizando **Vue.js** y **Vuetify**.

## Descripción

La aplicación tiene los siguientes componentes:

- **Lista de tareas**: Muestra todas las tareas con su título y estado (completada/no completada).
- **Ver tarea**: Permite ver los detalles de una tarea en un modal.
- **Eliminar tarea**: Permite eliminar una tarea.
- **Agregar tarea**: Permite agregar nuevas tareas con varios campos (título, estado, fecha, etc.).

## Tecnologías utilizadas

- **Vue 2**: Framework para crear interfaces reactivas.
- **Vuetify**: Librería de componentes para Vue.js, que proporciona una interfaz de usuario material design.
- **Fetch API**: Para realizar solicitudes HTTP a la API.

## Requisitos previos

1. **Node.js**: Debes tener **Node.js** instalado en tu máquina. Puedes verificar si lo tienes ejecutando `node -v` en tu terminal.
2. **npm o yarn**: El gestor de paquetes debe estar instalado.

## Instalación

1. Clona este repositorio:

```bash
git clone https://github.com/Erick-Hernandez-Ortega/Gestor-Tareas
```
2. Navega a la carpeta del proyecto:

```bash
cd tu-repositorio
```
3. Instala las dependencias:

```bash
npm install o yarn install
```

## Estructura del Proyecto

- **`/src`**: Carpeta donde se encuentran los componentes de Vue.js.
  - **`/components/`**: Componentes como `ListTask`, `SeeTask`, `RemoveTask`.
  - **`constants.js`**: Configuración con el `token` y `endpoint` de la API.
- **`index.vue`**: Componente raíz donde se define el layout principal de la aplicación.

## Ejecución

Para ejecutar el proyecto de forma local:

1. En el terminal, navega hasta la raíz de tu proyecto.
2. Ejecuta el siguiente comando para iniciar el servidor de desarrollo:

```bash
yarn dev o npm run dev
```

## Componentes Principales

### `ListTask.vue`

Este componente es el encargado de mostrar todas las tareas. Contiene una lista de tareas utilizando el componente `v-list` de Vuetify, y dentro de cada tarea se muestra su estado (completada o no completada) con un ícono correspondiente. Además, se incluyen los botones para ver los detalles de la tarea y eliminarla. 

Dentro de este componente se realiza una solicitud a la API para obtener todas las tareas y se actualiza el estado local `tasks` para renderizarlas. También se usa el componente `SeeTask` para ver los detalles de la tarea en un modal y `RemoveTask` para eliminarla.

**Características principales:**
- Muestra una lista de tareas.
- Permite ver más detalles de una tarea.
- Permite eliminar una tarea.

### `SeeTask.vue`

Este componente se muestra como un modal cuando se hace clic en el ícono de "ver tarea". Recibe un `taskId` como prop y realiza una solicitud GET a la API para obtener los detalles de esa tarea específica. Los detalles se muestran en el modal y permiten al usuario ver información más detallada.

**Características principales:**
- Recibe un `taskId` como prop.
- Hace una solicitud a la API para obtener los detalles de la tarea.
- Muestra los detalles de la tarea en un modal.

### `RemoveTask.vue`

Este componente permite eliminar una tarea. Cuando se hace clic en el ícono de eliminar, se realiza una solicitud DELETE a la API para eliminar la tarea correspondiente utilizando su `taskId`. Después de eliminar la tarea, se actualiza la lista de tareas.

**Características principales:**
- Permite eliminar tareas al hacer clic en un ícono.
- Hace una solicitud DELETE a la API para eliminar la tarea.
- Actualiza la lista de tareas después de la eliminación.

### `constants.js`

Este archivo contiene la configuración de la API, incluyendo el `token` de autenticación y la URL del endpoint de la API. Este archivo es utilizado en varios componentes para realizar las solicitudes a la API. 

**Contenido principal:**
- Define el `token` para autenticación en la API.
- Define la URL del `endpoint` de la API para realizar solicitudes.
