<script setup>
import { ref } from 'vue';

const isSidebarActive = ref(false);
const task = ref({
  id: null,
  name: '',
  time: '',
});
const errors = ref({ name: '', time: '' });

function closeSidebar() {
  isSidebarActive.value = false;
}

function submitTask() {
  if (!task.value.name) {
    errors.value.name = 'Name cannot be empty';
  } else {
    errors.value.name = '';
  }

  if (!task.value.time) {
    errors.value.time = 'Time cannot be empty';
  } else {
    errors.value.time = '';
  }

  if (!task.value.name || !task.value.time) {
    return;
  }

  // Emit an event to parent component to handle task submission
  emit('submit', task.value);
  isSidebarActive.value = false;
}
</script>

<template>
  <div
    class="fixed top-0 right-0 z-40 w-96 h-screen p-4 overflow-y-auto transition-transform bg-white"
    :class="isSidebarActive ? 'transform-none' : 'translate-x-full'"
  >
    <h5 class="text-base font-semibold text-gray-500 uppercase">Menu</h5>

    <!-- Close button -->
    <button
      type="button"
      @click="closeSidebar"
      class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 absolute top-2.5 end-2.5 inline-flex items-center"
    >
      <svg
        aria-hidden="true"
        class="w-5 h-5"
        fill="currentColor"
        viewBox="0 0 20 20"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
          clip-rule="evenodd"
        ></path>
      </svg>
      <span class="sr-only">Close menu</span>
    </button>

    <!-- Task form -->
    <div class="py-4 overflow-y-auto border-t mt-5">
      <form @submit.prevent="submitTask">
        <div class="mb-5">
          <label for="task-name"> Task name </label>
          <input
            type="text"
            id="task-name"
            v-model="task.name"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
            placeholder="Name here ..."
          />
          <p
            id="filled_error_help"
            v-show="errors.name"
            class="mt-2 text-xs text-red-600 dark:text-red-400"
          >
            {{ errors.name }}.
          </p>
        </div>
        <div class="mb-5">
          <label for="time">time</label>
          <input
            type="number"
            id="time"
            v-model="task.time"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
            placeholder="Time here ..."
          />
          <p
            id="filled_error_help"
            v-show="errors.time"
            class="mt-2 text-xs text-red-600 dark:text-red-400"
          >
            {{ errors.time }}.
          </p>
        </div>
        <button
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center"
        >
          Submit
        </button>
      </form>
    </div>
  </div>
</template>


