<script setup>
defineProps(['items', 'title']);
const emit = defineEmits(['delete']);

const importanceOptions = [
    {value: 'high', label: 'High'},
    {value: 'medium', label: 'Medium'},
    {value: 'low', label: 'Low'},
];
</script>
<template>
    <h1>{{ title }}</h1>
    <ul>
        <li
            v-for="item in items"
            :key="item.id"
            class="box is-align-items-center is-flex"
            :class="`importance-${item.importance}`"
        >
            <span>{{ item.text }}</span>

            <div class="ml-auto is-flex is-align-items-center item-actions">
                <label class="importance-label" :for="`importance-${item.id}`">Importance</label>
                <select :id="`importance-${item.id}`" v-model="item.importance" class="select is-small">
                    <option v-for="option in importanceOptions" :key="option.value" :value="option.value">
                        {{ option.label }}
                    </option>
                </select>
                <input type="checkbox" v-model="item.isDone">
                <button
                    class="delete is-medium"
                    type="button"
                    :aria-label="`Delete ${item.text}`"
                    @click="emit('delete', item.id)"
                ></button>
            </div>
        </li>
    </ul>
</template>