<template>
  <main>
    <section class="greetings">
      <h2 class="title">What's up<input :type="text" placeholder="Name Here" v-model="name"></h2>
      <form @submit.prevent="addTodo" type="submit">
      <input :type="text" v-model="newTodo" required placeholder="Nová úloha" class="ulohy">
      <button class="create">Pridať úlohu</button>
    </form>
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" class="checkbox">
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button v-on:click="removeTodo(todo)" class="removeTodo">X</button>
      </li>
    </ul>
    <button v-on:click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? 'Ukazat všechny' : 'Skryt dokončené' }}
    </button>

    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue';

// FUNCTION: If you write your name into input, after refresh it stays

const name = ref('')

watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})

// TODO LIST
  
let id = 0;

const newTodo = ref('');
const hideCompleted = ref(false);
const todos = ref([
  {id: id++, text: 'Kupiť baklažan'},
  {id: id++, text: 'nezomrieť'}
])

const filteredTodos = computed(() => {
  return hideCompleted.value
  ? todos.value.filter((t) => !t.done)
  : todos.value
})

function addTodo(){
  todos.value.push({ id: id++, text: newTodo.value, done: false })
}
newTodo.value = ''

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
}


</script>

