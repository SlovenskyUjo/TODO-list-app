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

<style scooped>
:root {
	--primary: #EA40A4;
	--business: #3A82EE;
	--business-glow: #3a71c4;
	--personal: var(--primary);
	--light: #EEE;
	--grey: #888;
	--dark: #313154;
	--danger: #ff5b57;

	--shadow: 0 1px 3px rgba(0, 0, 0, 0.1);

	--personal-glow: 0px 0px 4px rgba(234, 64, 164, 0.75);
}

* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'montserrat', sans-serif;
}

main {
  background: #000;
  width: 100vw;
  height: 100vh;
  color: var(--light);
  display: flex;
  justify-content: center;
  align-items: center;
}

.greetings {
  border: 4px solid var(--business);
  padding: 10rem 5rem;
}

.greetings input {
  margin-left: 1rem;
  display: inline;
  margin-bottom: 2rem;
}

input {
  outline: none;
  border: none;
  background-color: #000;
  border-bottom: 2px solid var(--light);
  color: var(--primary);
  font-size: 20px;
  font-weight: bold;
}

.ulohy {
  color: white;
  margin-right: 2rem
}

.create {
  padding: 8px 5px;
  border-radius: 15px;
  font-size: 20px;
  outline: none;
  border: none;
  background: var(--business);
}

.create:hover {
  background: var(--business-glow);
  transition: 0.2s ease-out;
}

.removeTodo {
  margin-left: 10px;
  /* background: #000; */
  outline: none;
  padding: 5px;
  border-radius: 5rem;
  border: none;
  border: 4px double var(--danger);
  font-size: 15px;
  font-weight: bold;
}

.done {
  text-decoration: line-through;
}

ul li {
  list-style: none;
}

.checkbox {
  outline: none;
  border: none;
  margin-right: 10px;
}

@media screen and (max-width: 600px) {
  main {
    display: flex;
    flex-direction: column;
    text-align: center;
  }


  .title,
  .ulohy,
  input,
  .greetings {
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 5px;
    
  }

  .create {
    margin-bottom: 1rem;
  }

  .greetings {
    padding: 2rem 3rem;
  }
}
</style>

