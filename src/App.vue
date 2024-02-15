<template>
  <div class="flex justify-center items-center h-full">
    <div class="flex justify-center flex-col gap-4 w-1/2 px-4">
      <h1 class="text-center font-bold text-2xl">TO-DO List</h1>
      <div class="flex gap-3">
        <input
          class="flex-1 rounded outline-none focus:ring-4 focus:ring-cyan-500 px-3 py-1 text-slate-900"
          type="text"
          name="task"
          id="task"
          v-model="title"
          placeholder="Escreva uma nova tarefa"
        />
        <div class="flex gap-2">
          <button
            class="bg-green-600 hover:bg-green-700 px-4 py-2 rounded font-semibold flex gap-2 transition-colors duration-300 ease-in-out"
            @click="addTodo"
            title="Adicionar nova tarefa"
          >
            <Plus />
          </button>
          <button
            class="bg-red-500 px-4 py-2 rounded font-semibold flex gap-2 disabled:cursor-not-allowed disabled:bg-red-400"
            :disabled="!todos.length"
            @click="resetTasks"
            title="Limpar lista de tarefas"
          >
            <TrashIcon />
          </button>
        </div>
      </div>
      <div class="flex flex-col space-y-2 mt-5">
        <div
          class="flex gap-2 items-center"
          v-for="todo in todos"
          :key="todo.id"
        >
          <div
            v-show="todo.isEditing === false"
            class="flex-1 hover:cursor-pointer"
            :class="{ 'line-through': todo.done }"
            @click="toggledTask(todo.id)"
          >
            {{ todo.title }}
          </div>
          <input
            class="flex-1 rounded outline-none focus:ring-4 focus:ring-cyan-500 px-3 py-1 text-slate-900"
            type="text"
            name="task"
            id="task"
            v-model="todo.title"
            v-show="todo.isEditing === true"
          />
          <div class="flex gap-2">
            <button
              v-show="todo.isEditing === true"
              class="p-2 bg-green-600 hover:bg-green-700 rounded-md group transition-colors duration-300 ease-in-out"
              @click="saveTask(todo.id)"
            >
              <SaveIcon
                class="w-4 h-4 text-slate-200 group-hover:text-slate-100"
              />
            </button>
            <button
              v-show="todo.isEditing === false"
              class="p-2 bg-orange-400 hover:bg-orange-600 rounded-md group transition-colors duration-300 ease-in-out"
              @click="enableEditingMode(todo.id)"
            >
              <PenIcon
                class="w-4 h-4 text-slate-200 group-hover:text-slate-100"
              />
            </button>
            <button
              class="p-2 bg-red-500 hover:bg-red-600 rounded-md group transition-colors duration-300 ease-in-out"
              @click="deleteTask(todo.id)"
            >
              <TrashIcon
                class="w-4 h-4 text-slate-200 group-hover:text-slate-100"
              />
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { PenIcon, Plus, SaveIcon, TrashIcon } from "lucide-vue-next";
import { reactive, ref } from "vue";
interface ITodo {
  id: number;
  title: string;
  done: boolean;
  isEditing: boolean;
}

const title = ref<string>("");

const todos = reactive<ITodo[]>([
  { id: 1, title: "Item 1", done: false, isEditing: false },
  { id: 2, title: "Item 2", done: false, isEditing: false },
  { id: 3, title: "Item 3", done: true, isEditing: false },
]);

const addTodo = () => {
  if (title.value) {
    todos.push({
      id: todos.length + 1,
      title: title.value,
      done: false,
      isEditing: false,
    });
    title.value = "";
  }
};

const resetTasks = () => {
  todos.splice(0, todos.length);
};

const toggledTask = (id: number) => {
  todos.filter((todo) => {
    if (todo.id === id) {
      todo.done = !todo.done;
    }
  });
};

const deleteTask = (id: number) => {
  const index = todos.findIndex((todo) => todo.id === id);
  todos.splice(index, 1);
};

const enableEditingMode = (id: number) => {
  todos.filter((todo) => {
    if (todo.id === id) {
      todo.isEditing = true;
    }
  });
};

const saveTask = (id: number) => {
  todos.filter((todo) => {
    if (todo.id === id) {
      todo.isEditing = false;
    }
  });
};
</script>
