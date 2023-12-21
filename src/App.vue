<template>
    <main
        class="font-sans flex h-full w-full items-center justify-center bg-teal-50"
    >
        <div class="lg:max-w-[600px] m-4 w-full rounded bg-white p-6 shadow">
            <div class="mb-4">
                <h1 class="text-grey-darkest mb-4 text-2xl">
                    Todo List ({{ todos.length }})
                </h1>
                <div class="mt-4 flex">
                    <input
                        class="text-grey-darker mr-4 w-full appearance-none rounded border px-3 py-2 shadow"
                        placeholder="Add Todo"
                        v-model="input_content"
                    />
                    <button
                        class="flex-no-shrink text-teal border-teal hover:bg-teal rounded border-2 p-2 hover:bg-green-500 hover:text-white"
                        @click="addTodo()"
                    >
                        Add
                    </button>
                </div>
            </div>

            <div class="mb-4">
                <h2 class="mb-4">Things to be done</h2>

                <p class="mb-4" v-if="todos.length === 0">
                    You don't have any tasks to do. Add a task above.
                </p>

                <div
                    class="mb-2 flex flex-col rounded-lg bg-gray-100 p-4 shadow-sm"
                    v-for="todo in todos"
                    :key="todo.id"
                >
                    <TodoItem :todo="todo" @removeTodo="removeTodo" />
                </div>

                <div v-if="todos.length !== 0">
                    <button
                        class="mt-4 w-full rounded-lg bg-red-600 p-2 text-white"
                        @click="deleteAllTodos"
                    >
                        Delete all todos!
                    </button>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import TodoItem from './components/TodoItem.vue'
import { uid } from 'uid'

onMounted(() => {
    getTodos()
})

const todos = ref([])
const input_content = ref('')

const addTodo = () => {
    if (input_content.value === '') {
        return
    }

    const newTodo = {
        id: uid(),
        content: input_content.value,
        done: false,
        createdAt: Date.now(),
    }

    todos.value.push(newTodo)

    localStorage.setItem('todos', JSON.stringify([...todos.value]))

    input_content.value = ''
}

const removeTodo = (todo) => {
    const index = todos.value.indexOf(todo)
    todos.value.splice(index, 1)

    localStorage.setItem('todos', JSON.stringify([...todos.value]))
}

const deleteAllTodos = () => {
    todos.value = []
    localStorage.removeItem('todos')
}

const getTodos = () => {
    const storedTodos = JSON.parse(localStorage.getItem('todos'))

    if (!storedTodos) {
        return
    }

    todos.value = storedTodos
}

const sortedTodos = computed(() => {
    return [...todos.value].sort((a, b) => {
        const titleA = a.content.toLowerCase()
        const titleB = b.content.toLowerCase()

        if (titleA < titleB) {
            return -1
        }
        if (titleA > titleB) {
            return 1
        }
        return 0
    })
})
</script>
