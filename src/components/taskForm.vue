<script lang="ts" setup>
import { ref } from 'vue';

const emit = defineEmits<{
    addTask : [newTask: string]
}>()
const newTask = ref("")
const error = ref("")

function submitForm() {
    if(newTask.value) {
        emit("addTask", newTask.value)
        newTask.value = ""
        error.value = ""
    } else {
        error.value = "Task cannot be empty"
    }
}
</script>

<template>
    <form @submit.prevent="submitForm">
        <label for="newTask">
            New Task
            <input type="text" id="newTask" autocomplete="off" :aria-invalid="!!error || undefined" v-model="newTask" @input="error = ''" />
            <small v-if="error" id="invalid-helper">
                {{ error }}
            </small>
        </label>

        <div class="button-container">
            <button class="outline">Add Task</button>
        </div>
    </form>
</template>