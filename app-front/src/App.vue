<template>
    <div class="app">
        <h1>Mi Lista de Tareas ✅</h1>

        <form @submit.prevent="addTask">
            <input v-model="newTask" placeholder="Escribe una tarea..." />
            <button type="submit">Agregar</button>
        </form>

        <h2>Tareas Pendientes</h2>
        <ul>
            <li v-for="task in tasks" :key="task.id" :class="{ done: task.completed }">
                <span @click="toggleDone(task.id)">
                    {{ task.title }}
                </span>
                <div class="task-actions">
                    <button @click="completeTask(task.id)" v-if="!task.completed" class="check-btn">✔️</button>
                    <button @click="removeTask(task.id)" class="delete-btn">❌</button>
                </div>
            </li>
        </ul>

        <h2>Tareas Completadas</h2>
        <ul>
            <li v-for="task in completedTasks" :key="task.id" :class="{ done: task.completed }">
                <span>
                    {{ task.title }}
                </span>
                <div class="task-actions">
                    <button @click="uncompleteTask(task.id)" class="uncheck-btn">⬆️</button>
                    <button @click="removeTask(task.id)" class="delete-btn">❌</button>
                </div>
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
            completedTasks: [],
        };
    },
    created() {
        this.getTasks();
    },
    methods: {
        getTasks() {
            axios
                .get("http://127.0.0.1:8000/api/tasks")
                .then((response) => {
                    this.tasks = response.data.filter((task) => !task.completed);
                    this.completedTasks = response.data.filter((task) => task.completed);
                })
                .catch((error) => {
                    console.error("Hubo un error al obtener las tareas:", error);
                });
        },

        addTask() {
            if (this.newTask.trim() !== "") {
                axios
                    .post("http://127.0.0.1:8000/api/tasks", {
                        title: this.newTask.trim(),
                        description: "Descripción de la tarea",
                        completed: false,
                    })
                    .then((response) => {
                        this.tasks.push(response.data);
                        this.newTask = "";
                    })
                    .catch((error) => {
                        console.error("Hubo un error al agregar la tarea:", error);
                    });
            }
        },

        toggleDone(taskId) {
            const task = this.tasks.find((task) => task.id === taskId);
            axios
                .put(`http://127.0.0.1:8000/api/tasks/${taskId}`, {
                    completed: !task.completed,
                })
                .then(() => {
                    task.completed = !task.completed;
                    if (task.completed) {
                        this.completedTasks.push(task);
                        this.tasks = this.tasks.filter((t) => t.id !== taskId);
                    } else {
                        this.tasks.push(task);
                        this.completedTasks = this.completedTasks.filter((t) => t.id !== taskId);
                    }
                })
                .catch((error) => {
                    console.error("Hubo un error al actualizar la tarea:", error);
                });
        },

        completeTask(taskId) {
            const task = this.tasks.find((task) => task.id === taskId);
            axios
                .put(`http://127.0.0.1:8000/api/tasks/${taskId}`, {
                    completed: true,
                })
                .then(() => {
                    task.completed = true;
                    this.completedTasks.push(task);
                    this.tasks = this.tasks.filter((t) => t.id !== taskId);
                })
                .catch((error) => {
                    console.error("Hubo un error al completar la tarea:", error);
                });
        },

        uncompleteTask(taskId) {
            const task = this.completedTasks.find((task) => task.id === taskId);
            axios
                .put(`http://127.0.0.1:8000/api/tasks/${taskId}`, {
                    completed: false,
                })
                .then(() => {
                    task.completed = false;
                    this.tasks.push(task);
                    this.completedTasks = this.completedTasks.filter((t) => t.id !== taskId);
                })
                .catch((error) => {
                    console.error("Hubo un error al desmarcar la tarea:", error);
                });
        },

        removeTask(taskId) {
            axios
                .delete(`http://127.0.0.1:8000/api/tasks/${taskId}`)
                .then(() => {
                    this.tasks = this.tasks.filter((task) => task.id !== taskId);
                    this.completedTasks = this.completedTasks.filter((task) => task.id !== taskId);
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

.task-actions {
    display: flex;
    gap: 10px;
}

.check-btn {
    background-color: green;
    color: white;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
}

.uncheck-btn {
    background-color: rgb(33, 111, 206);
    color: white;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
}

.delete-btn {
    background-color: rgb(232, 74, 74);
    color: white;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
}
</style>
