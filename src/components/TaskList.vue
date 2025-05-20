<script lang="ts" setup>
import type { Task } from '../types'

const props = defineProps<{
    tasks: Task[]
}>();

const emit = defineEmits<{
    toggleDone: [id: string],
    removeTask: [id: string],
}>()
</script>

<template>
    <TransitionGroup class="task-list" name="tasks" tag="div">
        <article v-for="task in tasks" :key="task.id" class="task-remove">
            <label :for="task.id">
                <input
                    type="checkbox"
                    :checked="task.done"
                    :id="task.id"
                    @input="emit('toggleDone', task.id)"
                />
                <span :class="{'done' : task.done}">
                    {{ task.title }}
                </span>
            </label>

            <button class="outline" @click="emit('removeTask', task.id)">Remove</button>
        </article>
    </TransitionGroup>
</template>

<style scoped>
.task-list{
    margin-top: 1rem;
}

.done {
    text-decoration: line-through;
}

.task-remove {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.tasks-enter-active,
.tasks-leave-active {
    transition: all 0.5s ease;
}
.tasks-enter-from,
.tasks-leave-to {
    opacity: 0;
    transform: translateX(300px);
}
</style>