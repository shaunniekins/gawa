<script setup>
import { ref, onMounted, watch } from "vue";

useSeoMeta({
  title: () => "Gawa",
  description: () => "To do list app for Gawa",
  link: [
    {
      rel: "icon",
      type: "image/jpeg",
      href: "/assets/images/gawa-logo.jpeg",
    },
  ],
});

const newTask = ref("");
const tasks = ref([]);

onMounted(() => {
  const savedTasks = localStorage.getItem("tasks");
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
});

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem("tasks", JSON.stringify(newTasks));
  },
  { deep: true }
);

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.unshift({ text: newTask.value, completed: false });
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

const toggleCompletion = (task) => {
  task.completed = !task.completed;
};
</script>

<template>
  <div class="h-[100svh] w-screen flex flex-col bg-[#F8F8FF] overflow-hidden">
    <nav class="py-4 px-5 text-center flex border border-[#DADCE0]">
      <div class="container px-5 md:mx-auto h-full md:px-96">
        <h1 class="font-semibold text-xl text-purple-700 uppercase">Gawa</h1>
      </div>
      <!-- <div class="container mx-auto w-14 h-14 relative">
        <img
          class="object-cover rounded-full"
          src="/assets/images/gawa-logo.jpeg"
          alt="Gawa Logo" />
      </div> -->
    </nav>
    <div class="container px-5 md:mx-auto h-full md:px-96 pt-5 pb-20">
      <div class="flex flex-col h-[10%] justify-center">
        <input
          class="rounded-md px-4 py-3 shadow-md drop-shadow-md placeholder:text-gray-700 overflow-auto whitespace-normal"
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="New task" />
        <!-- <button @click="addTask">Add</button> -->
      </div>
      <div class="mt-10 w-full h-[80%] flex flex-col gap-2">
        <h1 class="text-sm">TASKS</h1>
        <!-- bg-white border border-[#DADCE0]   -->
        <div
          class="h-full rounded-md overflow-y-auto overflow-x-hidden px-2 py-2">
          <!-- <div class="w-52 relative">
            <img
              class="object-cover rounded-full"
              src="/assets/images/clipboard.svg"
              alt="Gawa Logo" />
          </div> -->

          <ul
            :class="
              tasks.length === 0 && 'h-full flex justify-center items-center'
            "
            class="w-full flex flex-col gap-3">
            <div
              v-if="tasks.length === 0"
              class="flex flex-col justify-center items-center gap-2">
              <img
                class="w-52 object-cover rounded-full opacity-55"
                src="/assets/images/clipboard.svg"
                alt="Gawa Logo" />
              <p class="text-sm text-gray-500">No tasks</p>
            </div>
            <li
              class="w-full py-2 flex justify-between items-center"
              v-for="(task, index) in tasks"
              :key="index">
              <div class="flex gap-2">
                <input
                  type="checkbox"
                  v-model="task.completed"
                  @change="toggleCompletion" />
                <span
                  class="overflow-auto whitespace-normal"
                  :class="{ 'line-through': task.completed }">
                  {{ task.text }}
                </span>
              </div>
              <button @click="deleteTask(index)">
                <!-- <Icon name="uil:github" color="black" /> -->
                <!-- <Icon icon="mdi-light:home" /> -->
                <Icon name="ic:outline-delete" color="purple" size="20" />
                <!-- <iconify-icon icon="material-symbols:delete-outline-sharp"></iconify-icon> -->
              </button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
