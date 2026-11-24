<script setup>
defineProps({
    type: {
        type: String,
        default: 'primary',
        validator: (value) =>
            ['primary', 'link', 'info', 'success', 'warning', 'danger'].includes(value),
    },
    title: {
        type: String,
        default: '',
    },
    message: {
        type: String,
        required: true,
    },
    dismissible: {
        type: Boolean,
        default: false,
    },
});

const emit = defineEmits(['close']);
</script>

<template>
    <article class="message" :class="`is-${type}`">
        <div v-if="title" class="message-header">
            <p>{{ title }}</p>
            <button
                v-if="dismissible"
                class="delete"
                type="button"
                :aria-label="`Close ${title}`"
                @click="emit('close')"
            ></button>
        </div>
        <div class="message-body">
            <button
                v-if="dismissible && !title"
                class="delete is-pulled-right"
                type="button"
                aria-label="Close message"
                @click="emit('close')"
            ></button>
            {{ message }}
        </div>
    </article>
</template>
