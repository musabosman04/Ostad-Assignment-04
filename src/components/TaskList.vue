<script setup>
import { reactive, ref } from "vue";
import TaskEditPopup from "./TaskEditPopup.vue";

const isSidebarActive = ref(false);
const taskName = ref(null);
const taskTime = ref(null);
const errors = ref([]);
const oldTask = ref(null);

const tasks = reactive([
  { id: 1, name: "Task 1", time: 30 },
  { id: 2, name: "Task 2", time: 40 },
  { id: 3, name: "Task 3", time: 60 },
  { id: 4, name: "Task 4", time: 45 },
  { id: 5, name: "Task 5", time: 50 },
]);

function removeTask(index) {
  tasks.splice(index, 1);
}

function editTask(id) {
  let task = tasks.find((item) => item.id == id);
  taskName.value = task.name;
  taskTime.value = task.time;
  oldTask.value = task;

  // Ensure isSidebarActive is set to true
  isSidebarActive.value = true;
}

function addNewTask(task) {
  if (!task.name || !task.time) {
    errors.value = [];
    if (!task.name) {
      errors.value.push({ name: "Name cannot be empty" });
    }
    if (!task.time) {
      errors.value.push({ time: "Time cannot be empty" });
    }
    return;
  }

  if (!task.id) {
    tasks.push({
      name: task.name,
      time: task.time,
      id: tasks.length + 1,
    });
  } else {
    let index = tasks.findIndex((item) => item.id == task.id);

    if (index !== -1) {
      tasks[index].name = task.name;
      tasks[index].time = task.time;
    }
  }

  taskName.value = "";
  taskTime.value = "";
  oldTask.value = null;
  isSidebarActive.value = false;
}
</script>
<template>
  <!-- component -->
  <div
    class="h-screen w-full flex items-center justify-center bg-teal-lightest font-sans"
  >
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-xxl">
      <!-- ================= Add task button ================= -->
      <div class="mb-4">
        <h1 class="text-grey-darkest">Todo List</h1>
        <button
          @click="isSidebarActive = true"
          class="w-full block p-2 border-2 rounded hover:border-green-500 hover:text-green-900 hover:bg-green-200"
        >
          Add Task
        </button>
      </div>

      <!-- ================= Task table ================= -->
      <div>
        <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
          <table
            class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
          >
            <thead
              class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
            >
              <tr>
                <th scope="col" class="px-6 py-3">SL</th>
                <th scope="col" class="px-6 py-3">Task Name</th>
                <th scope="col" class="px-6 py-3">Time</th>
                <th scope="col" class="px-6 py-3 text-end">Action</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(task, index) in tasks"
                :key="index"
                class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700"
              >
                <th
                  class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
                >
                  {{ index + 1 }}
                </th>
                <td class="px-6 py-4">
                  {{ task.name }}
                </td>
                <td class="px-6 py-4">
                  <span class="me-1">{{ task.time }} </span>
                  <span>{{ task.time <= 1 ? "Minute" : "Minutes" }}</span>
                </td>
                <td class="px-6 py-4 text-end">
                  <button
                    @click="editTask(task.id)"
                    class="p-2 ml-2 border-2 rounded hover:border-yellow-500 hover:text-yellow-900 hover:bg-yellow-200"
                  >
                    Edit
                  </button>
                  <button
                    @click="removeTask(index)"
                    class="p-2 ml-2 border-2 rounded hover:border-red-500 hover:text-red-900 hover:bg-red-200"
                  >
                    Remove
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- ================= Task Edit Popup ================= -->
    <!-- At the top of your script setup block -->

    <!-- In your template -->
    <TaskEditPopup
      :isSidebarActive="isSidebarActive"
      :task="{
        id: oldTask ? oldTask.id : null,
        name: taskName,
        time: taskTime,
      }"
      :errors="errors"
      @submit="addNewTask"
      @closeSidebar="isSidebarActive = false"
    />

    <!-- Transparent layer -->
    <div
      class="bg-gray-900/50 dark:bg-gray-900/80 fixed inset-0 z-30"
      @click="isSidebarActive = false"
      v-show="isSidebarActive"
    ></div>
  </div>
</template>

<style scoped></style>
