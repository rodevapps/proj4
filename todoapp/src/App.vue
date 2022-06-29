<script setup lang="ts">
 import { ref } from 'vue';

 const newTodo = ref('');
  const defaultData = [{done: false, content: 'Write a blog post'}];
  const todosData = JSON.parse(localStorage.getItem('todos') as string) || defaultData;

  const todos = ref(todosData);

  function addTodo () {
   if (newTodo.value) {
    todos.value.push({done: false, content: newTodo.value});

    newTodo.value = '';
   }

   saveData();
  }

  function doneTodo (todo: {done: boolean}) {
   todo.done = !todo.done
   saveData();
  }

  function removeTodo (index: number) {
   todos.value.splice(index, 1);
   saveData();
  }

  function saveData () {
   const storageData = JSON.stringify(todos.value);
   localStorage.setItem('todos', storageData);
  }
</script>

<template>
 <h1>ToDo App</h1>

 <form @submit.prevent="addTodo()">
  <label>New ToDo</label>
  <input v-model="newTodo" name="newTodo" autocomplete="off">
  <button>Add ToDo</button>
 </form>

 <h1>ToDo List</h1>

 <ul>
  <li v-for="(todo, index) in todos" :key="index">
   <span :class="{ done: todo.done }" @click="doneTodo(todo)">{{ todo.content }}</span>
   <button @click="removeTodo(index)">Remove</button>
  </li>
 </ul>

 <h1 v-if="todos.length === 0">Empty list.</h1>
</template>

<style lang="scss">
</style>
