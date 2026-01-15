<template>
    <div class="relative">
        <!-- Input задачи с динамической рамкой -->
        <input type="text" class="form-control form-control-lg padding-right-lg"
            placeholder="+ Add new task. Press enter to save." @keydown.enter="addNewTask" ref="inputRef"
            v-model="newTask.name" :class="priorityColorClass" />

        <!-- Флаг выбранного приоритета -->
        <span v-if="selectedPriority" class="badge priority-badge" :class="selectedPriority.color">
            {{ selectedPriority.name }}
        </span>

        <!-- Селект приоритета -->
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
    priority_id: null,
});

const inputRef = ref();
const selectedPriority = ref(null);

// Карта цветов для приоритетов
const priorityMap = {
    1: "border-danger text-bg-danger", // high
    2: "border-warning text-bg-warning", // medium
    3: "border-primary text-bg-primary", // low
    null: "border-secondary", // default / none
};

// Класс для подсветки рамки input
const priorityColorClass = computed(() => {
    return selectedPriority.value?.color ?? "border-secondary";
});

// Выбор приоритета
const setPriority = (priority) => {
    // priority = { id, name, color } из SelectPriority
    selectedPriority.value = priority;
    newTask.priority_id = priority.id;
    inputRef.value.focus();
};

// Добавление новой задачи
const addNewTask = async () => {
    if (newTask.name.trim()) {
        await handleAddedTask(newTask);
        newTask.name = "";
        newTask.priority_id = null;
        selectedPriority.value = null;
    }
};
</script>

<style>
.relative {
    position: relative;
}

/* Селект приоритета */
.select-priority {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    right: 10px;
    padding-left: 10px;
    z-index: 999;
}

/* Флаг выбранного приоритета */
.priority-badge {
    position: absolute;
    top: 50%;
    right: 100px;
    /* немного левее селекта */
    transform: translateY(-50%);
    padding: 0.3rem 0.5rem;
    font-size: 0.8rem;
    pointer-events: none;
    z-index: 999;
}
</style>
