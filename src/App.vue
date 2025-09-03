<script setup>
import { computed, reactive } from 'vue'

const todos = reactive([
  { id: 1, text: 'Изучить Vue', completed: false },
  { id: 2, text: 'Пример задачи', completed: false },
  { id: 3, text: 'Изучить React', completed: false },
  { id: 4, text: 'Создать ToDo List', completed: false },
])

const checkClick = (item) => {
  item.completed = true
}

const removeTodo = (id) => {
  const index = todos.findIndex((item) => item.id === id)
  todos.splice(index, 1)
}

const clearCompleted = () => {
  for (let i = todos.length - 1; i >= 0; i--) {
    if (todos[i].completed) {
      todos.splice(i, 1)
    }
  }
}

const clearAll = () => {
  todos.splice(0, todos.length)
}

const notCompletedTodo = computed(() => {
  const copy = [...todos]
  const len = copy.filter((item) => !item.completed).length
  return len
})
</script>

<template>
  <body>
    <div class="container todo-app">
      <h1 class="title">Todo List</h1>

      <div class="todo-app__main">
        <ul class="todo-list">
          <li
            v-for="item in todos"
            :key="item.id"
            class="todo-list__item"
            :class="{
              'todo-list__item--completed': item.completed,
            }"
          >
            {{ item.text }}
            <button
              :disabled="item.completed"
              @click="checkClick(item)"
              class="btn btn--check"
              aria-label="Завершить"
            >
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" width="15" height="22">
                <path
                  d="M438.6 109.4c-12.5-12.5-32.8-12.5-45.3 0L160 320.7l-92.3-92.3c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l112 112c12.5 12.5 32.8 12.5 45.3 0l288-288c12.6-12.5 12.6-32.8 .1-45.3z"
                />
              </svg>
            </button>
            <button @click="removeTodo(item.id)" class="btn btn--delete" aria-label="Удалить">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" width="15" height="22">
                <path
                  d="M135.2 17.7L128 32 32 32C14.3 32 0 46.3 0 64S14.3 96 32 96l384 0c17.7 0 32-14.3 32-32s-14.3-32-32-32l-96 0-7.2-14.3C307.4 6.8 296.3 0 284.2 0L163.8 0c-12.1 0-23.2 6.8-28.6 17.7zM416 128L32 128 53.2 467c1.6 25.3 22.6 45 47.9 45l245.8 0c25.3 0 46.3-19.7 47.9-45L416 128z"
                />
              </svg>
            </button>
          </li>
        </ul>
        <div class="todo-list__empty" v-if="todos.length === 0">
          <p>Список задач пуст</p>
        </div>
      </div>

      <div class="todo-app__footer" v-if="todos.length !== 0">
        <p class="todo-app__footer-text">Осталось {{ notCompletedTodo }} задания(й)</p>
        <button @click="clearCompleted()" class="btn btn--clear">Удалить завершенные</button>
        <button @click="clearAll()" class="btn btn--clear">Очистить список</button>
      </div>
    </div>
  </body>
</template>

<style src="./App.css"></style>
