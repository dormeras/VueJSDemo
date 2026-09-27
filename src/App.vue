<script setup>
import { ref } from 'vue'

// Реактивные переменные
const text = ref('')
const tasks = ref([
  { id: 1, text: 'Изучить Vue.js', completed: true },
  { id: 2, text: 'Подготовить доклад', completed: false }
])

// Функция добавления задачи
function addTask() {
  const value = text.value.trim()
  if (!value) return

  tasks.value.push({
    id: Date.now(),
    text: value,
    completed: false
  })

  text.value = '' // Очищаем поле ввода
}

// Функция удаления задачи
function deleteTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}
</script>

<template>
  <div class="todo-app">
    <h1>Список задач</h1>

    <!-- Форма ввода задачи -->
    <div class="input-group">
      <input 
        v-model="text" 
        @keyup.enter="addTask"
        placeholder="Новая задача..." 
      />
      <button @click="addTask">Добавить</button>
    </div>

    <!-- Условный вывод: если список пуст -->
    <p v-if="!tasks.length" class="empty-msg">Список пока пуст</p>

    <!-- Вывод списка задач -->
    <ul v-else class="task-list">
      <li v-for="task in tasks" :key="task.id" class="task-item">
        <label :class="{ done: task.completed }">
          <input type="checkbox" v-model="task.completed">
          <span>{{ task.text }}</span>
        </label>
        <button class="delete-btn" @click="deleteTask(task.id)">✕</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

h1 {
  text-align: center;
  color: #333;
  margin-top: 0;
}

.input-group {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 8px 12px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 8px 16px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

button:hover {
  background-color: #33a06f;
}

.empty-msg {
  text-align: center;
  color: #888;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

.task-item label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.task-item label.done span {
  text-decoration: line-through;
  color: #888;
}

.delete-btn {
  background: none;
  color: #ff5c5c;
  padding: 4px 8px;
  font-size: 14px;
  border: none;
  cursor: pointer;
}

.delete-btn:hover {
  background-color: #ffeeee;
}
</style>