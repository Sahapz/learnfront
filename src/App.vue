<script setup>
import { ref } from 'vue';

let message = ref('Hello vue!');

let items = ref(['Item 1', 'Item 2', 'Item 3']);
 function add() {

    if (message.value.trim() === '') {
        return;
    }
    items.value.push(message.value.trim());
    message.value = '';
   }    
</script>


<template>
    <div class="container content mt-2">


        <!--input v-bind:value="message" v-on:input="message = $event.target.value" class="input" type="text" placeholder="Sisesta sõnum"-->
        <div class="field has-addons">
            <div class="control is-expanded">
                <input @keyup.enter="add" type="text" v-model="message" class="input" placeholder="Add item">
            </div>
            <div class="control">
                <button @click="add" class="button is-info">Add</button>
            </div>
        </div>
        <ul>
            <li v-for="item in items" :key="item">{{ item }}</li>
        </ul>
    </div>
</template>


<style></style>
