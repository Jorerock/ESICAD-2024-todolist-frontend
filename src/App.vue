<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoComponent from './components/TodoComponent.vue';
import { Todo } from '@/models/todo';

const monTableau = ref<Todo[]>([]);

onMounted(async () => {
  const todosRequest = await fetch('http://localhost:3000/todos');
  const todos:Todo[] = await todosRequest.json();
  monTableau.value = [...todos];
});

// const monTableau = [1, 2, 3];

const onTodoInput = async (newTodoValue: Todo, index: number) => {
  monTableau.value[index] = newTodoValue;
  await fetch(`http://localhost:3000/todos/${newTodoValue.id}`, {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(newTodoValue),
  });
  console.log('monTableau est mis à jour et la modification est envoyée au serveur');
};

const deleteTodo = async (id: number, index: number) => {
  await fetch(`http://localhost:3000/todos/${id}`, {
    method: 'DELETE',
  });
  monTableau.value.splice(index, 1);
  console.log('Todo supprimé');
};

// const addTodo = async (id: number, index: number) => {
// // Ajout du todo sur le serveur
// await fetch(`http://localhost:3000/todos/${id}`, {
// method: 'POST',
// });
// // Mise à jour locale du tableau
// monTableau.value.splice(index, 1);
// console.log('Todo Ajoute');
// };



</script>

<template>
  <p>Hello World !</p>
  <span v-if="monTableau.length % 2 === 0">Mon tableau est
  pair</span>
  <span v-else>Mon tableau est impair</span>
  <br />
  <TodoComponent
  v-for="(element, index) in monTableau"
  :todo="element"
  v-bind:key="element.id"
  @onInput="onTodoInput($event, index)"
  />
  <button @click="deleteTodo(element.id, index)">Supprimer</button>
  </template>
  

<style scoped></style>