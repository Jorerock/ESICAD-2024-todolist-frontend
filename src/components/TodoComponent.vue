<script setup lang="ts">
import { ref } from 'vue';


const props = defineProps<{
    todo: {
        done: boolean,
        todo: string
    }
}>();

const editMode = ref(false);

const newValue = ref(props.todo.todo);

const emit = defineEmits(['onInput'])
const onInput = (value: boolean) => {
    console.log('TodoComponent a détecté un changement ', value);
    emit('onInput', { ...props.todo, done: value })
}

const onConfirmText = () => {
    editMode.value = false;
    emit('onInput', { ...props.todo, todo: newValue.value });
}
const onCancelText = () => {
    editMode.value = false;
    newValue.value = props.todo.todo;
}
</script>

<template>
    <span v-if="!editMode">
        <span @click="editMode = !editMode">
            {{ props.todo.todo }}
        </span>
        <input type="checkbox" :checked="props.todo.done" @click="(event: any) => onInput(event.target?.checked)" />
        <br />
    </span>
    <span v-else>
        <!-- mode edition -->
        <input type="text" v-model="newValue" />
        <button @click="onConfirmText">Confirmer</button>
        <button @click="onCancelText">Annuler</button>
        <br />
    </span>

</template>

<style lang="css" scoped>
.done {
    color: green;
}
</style>


<script setup lang="ts">
import { ref } from 'vue';
import { Todo } from '../models/Todo'; // Import de l'interface
Todo
const props = defineProps<{
todo: Todo;
}>();
const editMode = ref(false);
const newValue = ref(props.todo.label); // Utilisation de "label"
au lieu de "todo"
const emit = defineEmits(['onInput']);
const onInput = (value: boolean) => {
console.log('TodoComponent a détecté un changement', value);
emit('onInput', { ...props.todo, done: value });
};
const onConfirmText = () => {
editMode.value = false;
emit('onInput', { ...props.todo, label: newValue.value });
};
const onCancelText = () => {
editMode.value = false;
newValue.value = props.todo.label;
};
</script>

<template>
    <span v-if="!editMode">
    <span @click="editMode = !editMode">
    {{ props.todo.label }}
    </span>
    <input type="checkbox" :checked="props.todo.done" @click="(event:
    any) => onInput(event.target?.checked)" />
    <br />
    </span>
    <span v-else>
    <!-- mode édition -->
    <input type="text" v-model="newValue" />
    <button @click="onConfirmText">Confirmer</button>
    <button @click="onCancelText">Annuler</button>
    <br />
    </span>
    </template>