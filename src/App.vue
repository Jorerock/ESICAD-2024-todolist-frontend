<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoComponent from './components/TodoComponent.vue';
const monTableau = ref([]);

onMounted(async () => {
  const todosRequest = await fetch('http://localhost:3000/todos');
  const todos = await todosRequest.json();
  monTableau.value = [...todos];
});

// const monTableau = [1, 2, 3];

const onTodoInput = (newTodoValue: any, index: number) => {
  monTableau.value[index] = newTodoValue;
  console.log('monTableau est mis à jour');
}

</script>

<template>
  <p>Hello World !</p>
  <span v-if="monTableau.length % 2 === 0">Mon tableau est pair</span>
  <span v-else>Mon tableau est impair</span>
  <br />
  <TodoComponent v-for="(element, index) in monTableau" :todo="element" v-bind:key="index"
    @onInput="onTodoInput($event, index)" />

  <!-- {{ monTableau }} -->

</template>

<style scoped></style>


<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoComponent from './components/TodoComponent.vue';
import { Todo } from './models/Todo'; // Import de l'interface Todo


const monTableau = ref<Todo[]>([]);
onMounted(async () => {
const todosRequest = await fetch('http://localhost:3000/todos');
const todos: Todo[] = await todosRequest.json();
monTableau.value = [...todos];
});


const onTodoInput = async (newTodoValue: Todo, index: number) => {
monTableau.value[index] = newTodoValue;
// Appel PUT pour mettre à jour le todo côté serveur
await fetch(`http://localhost:3000/todos/${newTodoValue.id}`, {
method: 'PUT',
headers: {
'Content-Type': 'application/json',
},
body: JSON.stringify(newTodoValue),
});
console.log('monTableau est mis à jour et la modification est
envoyée au serveur');
};

const deleteTodo = async (id: number, index: number) => {
// Suppression sur le serveur
await fetch(`http://localhost:3000/todos/${id}`, {
method: 'DELETE',
});
// Mise à jour locale du tableau
monTableau.value.splice(index, 1);
console.log('Todo supprimé');
};


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