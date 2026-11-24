<script setup>
import { computed, ref } from 'vue';
import ItemList from './ItemList.vue';
import Message from './Message.vue';

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

const messages = ref([
    { id: 1, type: 'primary', title: 'Primary', message: 'This is a primary Bulma message.', visible: true },
    { id: 2, type: 'info', title: 'Information', message: 'Props are validated by the reusable Message component.', visible: true },
    { id: 3, type: 'success', title: 'Success', message: 'The demo message was dismissed successfully.', visible: true },
    { id: 4, type: 'warning', title: 'Warning', message: 'Check your input before continuing.', visible: true },
    { id: 5, type: 'danger', title: 'Danger', message: 'Something went wrong.', visible: true },
]);

function closeMessage(id) {
    const messageItem = messages.value.find((item) => item.id === id);
    if (messageItem) {
        messageItem.visible = false;
    }
}

function resetMessages() {
    messages.value.forEach((item) => {
        item.visible = true;
    });
}
</script>

<template>
    <div class="container content mt-2">
        <section class="mb-5">
            <h1>Reusable Message demo</h1>

            <div class="mb-3">
                <button class="button is-small" type="button" @click="resetMessages">
                    Reset messages
                </button>
            </div>

            <Message
                v-for="item in messages"
                v-show="item.visible"
                :key="item.id"
                :type="item.type"
                :title="item.title"
                :message="item.message"
                dismissible
                @close="closeMessage(item.id)"
            ></Message>
        </section>

        <hr>

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