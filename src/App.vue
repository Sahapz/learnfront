<script setup>
import { computed, ref } from 'vue';
import ItemList from './ItemList.vue';

let message = ref('');
let i = 1;
let items = ref([
    {id: i++, text: 'Piim', isDone: false, importance: 'high'},
    {id: i++, text: 'Viin', isDone: true, importance: 'low'},
    {id: i++, text: 'Õlu', isDone: false, importance: 'medium'},
    {id: i++, text: 'Sai', isDone: true, importance: 'high'},
]);

function add(){
  if(message.value.trim() !== '') {
        items.value.push({id: i++, text: message.value.trim(), isDone: false, importance: 'medium'});
  }
  message.value = '';
}

function removeItem(itemId) {
        items.value = items.value.filter(item => item.id !== itemId);
}

const importanceRank = {high: 3, medium: 2, low: 1};
function sortByImportance(itemList) {
    return [...itemList].sort((first, second) =>
        importanceRank[second.importance] - importanceRank[first.importance]
    );
}

let allItems = computed(() => sortByImportance(items.value));
let doneItems = computed(() => sortByImportance(items.value.filter(item => item.isDone)));
let toDoItems = computed(() => sortByImportance(items.value.filter(item => !item.isDone)));
</script>

<template>
    <div class="container content mt-2">
        <div class="field has-addons">
            <div class="control is-expanded">
                <input @keydown.enter="add" v-model="message" class="input" type="text" placeholder="Add item">
            </div>
            <div class="control">
                <button @click="add" class="button is-info">
                    Add
                </button>
            </div>
        </div>

        <ItemList :items="allItems" title="All items" @delete="removeItem"></ItemList>
        <ItemList :items="doneItems" title="Done items" @delete="removeItem"></ItemList>
        <ItemList :items="toDoItems" title="ToDo items" @delete="removeItem"></ItemList>
    </div>
</template>

<style></style>