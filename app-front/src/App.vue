<template>
    <div class="app">
        <h1>Mi Lista de Tareas ✅</h1>

        <form @submit.prevent="addTask">
            <input v-model="newTask" placeholder="Escribe una tarea..." />
            <button type="submit">Agregar</button>
        </form>

        <ul>
            <li v-for="task in tasks" :key="task.id" :class="{ done: task.completed }">
                <span @click="toggleDone(task.id)">
                    {{ task.title }}
                </span>
                <button @click="removeTask(task.id)">❌</button>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            newTask: "",
            tasks: [],
        };
    },
    created() {
        this.getTasks();
    },
    methods: {
        // Obtener todas las tareas desde la API
        getTasks() {
            axios
                .get("http://127.0.0.1:8000/api/tasks")
                .then((response) => {
                    this.tasks = response.data;
                })
                .catch((error) => {
                    console.error("Hubo un error al obtener las tareas:", error);
                });
        },

        // Agregar una nueva tarea a la API
        addTask() {
            if (this.newTask.trim() !== "") {
                axios
                    .post("http://127.0.0.1:8000/api/tasks", {
                        title: this.newTask.trim(),
                        description: "Descripción de la tarea",
                        completed: false,
                    })
                    .then((response) => {
                        this.tasks.push(response.data); // Añadir la nueva tarea al array
                        this.newTask = ""; // Limpiar el input
                    })
                    .catch((error) => {
                        console.error("Hubo un error al agregar la tarea:", error);
                    });
            }
        },

        // Marcar una tarea como completada o no
        toggleDone(taskId) {
            const task = this.tasks.find((task) => task.id === taskId);
            axios
                .put(`http://127.0.0.1:8000/api/tasks/${taskId}`, {
                    completed: !task.completed,
                })
                .then(() => {
                    task.completed = !task.completed; // Actualizar el estado de la tarea
                })
                .catch((error) => {
                    console.error("Hubo un error al actualizar la tarea:", error);
                });
        },

        // Eliminar una tarea
        removeTask(taskId) {
            axios
                .delete(`http://127.0.0.1:8000/api/tasks/${taskId}`)
                .then(() => {
                    this.tasks = this.tasks.filter((task) => task.id !== taskId);
                })
                .catch((error) => {
                    console.error("Hubo un error al eliminar la tarea:", error);
                });
        },
    },
};
</script>

<style>
.app {
    max-width: 600px;
    margin: 2rem auto;
    padding: 1rem;
    font-family: Arial, sans-serif;
}

input {
    padding: 0.5rem;
    margin-right: 0.5rem;
    width: 70%;
}

button {
    padding: 0.5rem;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin: 0.5rem 0;
    background: #f2f2f2;
    padding: 0.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.done {
    text-decoration: line-through;
    color: gray;
}
</style>
