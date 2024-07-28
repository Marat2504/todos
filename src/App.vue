<template>
    <main class="container m-auto">
        <h1 class="text-pink-600 text-9xl text-center">todos</h1>

        <input

                v-model="text"
                @keydown.enter="handleEnter"
                class="border-2 border-gray-200 px-20 py-2 mt-5 text-3xl w-full text-start"
                type="text" name="" id="" placeholder="What needs to be done?">

        <ul
                v-if="todo.length > 0"
                class="w-full">
            <li
                    v-for="t in todoList" :key="t.id"

                    @mouseover="handleMouseOver(t.id)"
                    @mouseleave="handleMouseLeave"
                    class="border-2 border-gray-200 px-20 py-2 text-3xl text-start relative"
                    :class="[t.completed === true ? 'text-gray-500 line-through' : 'text-black']"
            >
                <span
                        @click="handleSelect(t)"
                        v-if="t.completed === false"
                        class="inline-block w-10 h-10 text-center border-2 border-gray-200 rounded-full mr-5 cursor-pointer"></span>

                <span
                        @click="handleSelect(t)"
                        v-else
                        class="inline-block w-10 h-10 text-center border-2 border-green-400 rounded-full mr-5 text-green-400 cursor-pointer">&#10003;
                </span>
                {{ t.text }}
                <span
                        @click="handleDeleteElement(t)"
                        v-if="t.id === activeId"
                        class="absolute right-0 items-center justify-center h-full w-10 text-pink-600
                    text-3xl cursor-pointer">x</span>
            </li>
            <li class="flex justify-between px-5 py-2">
                <span class="mr-auto">{{ unfinishedGoal }} задач не выполнено</span>
                <div class="flex gap-10">
                    <span
                            @click="changeFilter('all')"
                            class="ml-2 border-2 px-2 py-2 rounded cursor-pointer
                            hover:border-red-200"
                            :class="[filter === 'all' ? 'border-red-500' : 'border-transparent']"
                    >
                        All
                    </span>

                    <span
                            @click="changeFilter('active')"
                            class="ml-2 border-2 px-2 py-2 rounded cursor-pointer
                            hover:border-red-200"
                            :class="[filter === 'active' ? 'border-red-500' : 'border-transparent']"
                    >
                        Active
                    </span>

                    <span
                            @click="changeFilter('complete')"
                            class="ml-2 border-2 px-2 py-2 rounded cursor-pointer
                            hover:border-red-200"
                            :class="[filter === 'complete' ? 'border-red-500' : 'border-transparent']"
                    >
                        Completed
                    </span>
                </div>
                <span
                        @click="clearCompleted"
                        class="ml-auto px-2 py-2 bg-red-200 cursor-pointer rounded transition
                        hover:bg-red-500"
                >Clear Completed</span>
            </li>

        </ul>

    </main>
</template>

<script setup>
import {computed, reactive, ref} from "vue";

let activeId = ref('')
let text = ref('')
let todo = reactive(Array())
let filter = ref('all')

const unfinishedGoal = computed(() => {
    let count = ref(0)
    for (let i = 0; i < todo.length; i++) {
        if (todo[i].completed === false) {
            count.value += 1
        }
    }
    return count
})

const handleEnter = () => {
    if (text.value.length > 0) {
        todo.push({
            id: Date.now(),
            text: text.value,
            completed: false
        })
        text.value = ''
    }
}

const handleMouseOver = (id) => {
    activeId.value = id
}

const handleMouseLeave = () => {
    activeId.value = ''
}

const handleDeleteElement = (element) => {
    return todo = todo.filter(el => el !== element)
}

const handleSelect = (element) => {
    const indexElement = todo.indexOf(element)
    if (indexElement !== -1) {
        return todo[indexElement].completed = !todo[indexElement].completed
    }
}

const changeFilter = (type) => filter.value = type

const clearCompleted = () => {
    return todo = todo.filter(el => el.completed === false)

}

const todoList = computed(() => {
    if (filter.value === 'all') {
        const todoListAll = [...todo]
        return todoListAll
    } else if (filter.value === 'active') {
        const todoListComputedActive = todo.filter(el => el.completed === false)
        return todoListComputedActive
    } else {
        const todoListComputedComplete = todo.filter(el => el.completed === true)
        return todoListComputedComplete
    }
})

</script>

<style>

</style>
