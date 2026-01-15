<template>
    <div class="relative">
        <input type="text" class="form-control form-control-lg padding-right-lg"
            placeholder="+ Add new task. Press enter to save." @keydown.enter="addNewTask" :class="inputClass"
            ref="inputRef" />
        <div class="select-priority">
            <SelectPriority @change="setPriority" />
        </div>
    </div>
</template>
<script setup>
import { reactive, ref, computed } from "vue";
import { useTaskStore } from "../../stores/task";
import SelectPriority from "./SelectPriority.vue";

const store = useTaskStore();
const { handleAddedTask } = store;

const newTask = reactive({
    name: "",
    is_completed: false,
    priority_id: null
});

const inputClass = computed(() => {
    return priorityColors[newTask.priority_id ?? null];
});

const inputRef = ref()

const setPriority = (id) => {
    console.log("Selected priority ID:", id);
    newTask.priority_id = id;
    inputRef.value.focus();
};

const priorityColors = {
    1: "border-danger",   // high
    2: "border-warning",  // medium
    3: "border-primary",  // low
    null: "border-secondary", // default / нет приоритета
};

const addNewTask = async (event) => {
    if (event.target.value.trim()) {
        newTask.name = event.target.value;
        event.target.value = "";
        await handleAddedTask(newTask);
    }
};
</script>
<style>
.relative {
    position: relative;
}

.select-priority {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    right: 10px;
    padding-left: 10px;
    z-index: 999;
}
</style>