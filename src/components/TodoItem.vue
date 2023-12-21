<template>
    <div class="flex items-center justify-between">
        <div>
            <h3 class="mb-4 border-b pb-4">{{ todo.content }}</h3>
            <p>{{ new Date(todo.createdAt).toDateString() }}</p>
        </div>

        <div>
            <button
                @click="removeTodo(todo)"
                class="cursor-pointer rounded-lg bg-red-500 p-2 text-white"
            >
                Delete
            </button>
        </div>
    </div>
</template>

<script setup>
import { defineEmits, defineProps } from 'vue'

const props = defineProps({
    todo: Object,
})

const emits = defineEmits(['removeTodo'])

const removeTodo = (todo) => {
    emits('removeTodo', todo)
}
</script>
