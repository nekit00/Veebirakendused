<script setup>
import { computed, ref } from 'vue';
import ItemList from './ItemList.vue';

// Olulisuse tasemed: mida suurem number, seda olulisem
const importanceLevels = [
    {value: 3, label: 'Kõrge'},
    {value: 2, label: 'Keskmine'},
    {value: 1, label: 'Madal'},
];

let message = ref('');
let importance = ref(2);
let i = 1;
let items = ref([
    {id: i++, text: 'Piim', isDone: false, importance: 2},
    {id: i++, text: 'Viin', isDone: true, importance: 1},
    {id: i++, text: 'Õlu', isDone: false, importance: 3},
    {id: i++, text: 'Sai', isDone: true, importance: 3},
    {id: i++, text: 'Juust', isDone: false, importance: 1},
    {id: i++, text: 'Kohv', isDone: true, importance: 2},
]);

function add(){
  if(message.value.trim() !== '') {
    items.value.push({
      id: i++,
      text: message.value.trim(),
      isDone: false,
      importance: Number(importance.value),
    });
  }
  message.value = '';
  importance.value = 2;
}

// Sorteeritud olulisuse järgi: kõige olulisem ees
let sortedItems = computed(() => [...items.value].sort((a, b) => b.importance - a.importance));
let doneItems = computed(() => [...items.value].filter(item => item.isDone).sort((a, b) => b.importance - a.importance));
let toDoItems = computed(() => [...items.value].filter(item => !item.isDone).sort((a, b) => b.importance - a.importance));
</script>

<template>
    <div class="container content mt-2">
        <div class="field has-addons">
            <div class="control is-expanded">
                <input @keydown.enter="add" v-model="message" class="input" type="text" placeholder="Add item">
            </div>
            <div class="control">
                <div class="select">
                    <select v-model="importance">
                        <option v-for="level in importanceLevels" :key="level.value" :value="level.value">
                            {{ level.label }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="control">
                <button @click="add" class="button is-info">
                    Add
                </button>
            </div>
        </div>

        <ItemList :items="sortedItems" title="All items"></ItemList>
        <ItemList :items="doneItems" title="Done items"></ItemList>
        <ItemList :items="toDoItems" title="ToDo items"></ItemList>
    </div>
</template>

<style></style>
