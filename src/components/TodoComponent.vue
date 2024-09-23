<script setup lang="ts">
import { ref } from 'vue';
import { Todo } from '../models/todo';

const props = defineProps<{ todo: Todo }>();

const editMode = ref(false);

const newValue = ref(props.todo.label);

const emit = defineEmits(['onInput'])
const onInput = (value: boolean) => {
    console.log('TodoComponent a détecté un changement ', value);
    emit('onInput', { ...props.todo, done: value })
}

const onConfirmText = () => {
    editMode.value = false;
    emit('onInput', { ...props.todo, label: newValue.value });
}
const onCancelText = () => {
    editMode.value = false;
    newValue.value = props.todo.label;
}
</script>

<template>
    <span v-if="!editMode">
        <span @click="editMode = !editMode">
            {{ props.todo.label }}
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