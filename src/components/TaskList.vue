<script setup>
import { reactive, ref } from 'vue';
const isSidebarActive = ref(false);

const taskName = ref(null);
const taskTime = ref(null);

const errors = ref([]);

const oldTask = ref(null);

// Task default data =========================================>
const tasks = reactive([
    { id: 1,name: 'Task 1', time: 30 },
    { id: 2,name: 'Task 2', time: 40 },
    { id: 3,name: 'Task 3', time: 60 },
    { id: 4,name: 'Task 4', time: 45 },
    { id: 5,name: 'Task 5', time: 50 },
]);

// Task remove function =========================================>
function removeTask(index) {
    tasks.splice(index, 1);
};

// Task Edit function =========================================>
function editTask(id) {
    let task = tasks.find(item => item.id == id);
    taskName.value = task.name;
    taskTime.value = task.time;
    oldTask.value = task;

    isSidebarActive.value = true;
};




// Add task function =========================================>
function addNewTask() {
    if (!taskName.value || !taskTime.value) {
        if (!taskName.value) {
            errors.value.push({
                name: 'Name cannot be empty'
            });
        } else {
            errors.value = [];
        }

        if (!taskTime.value) {
            errors.value.push({
                time: 'Time cannot be empty'
            });
        } else {
            errors.value = [];
        }
        return;
    }


    if (!oldTask.value) {
        tasks.push({
            name: taskName.value,
            time: taskTime.value,
            id: tasks.length + 1
        });
    } else {
        let index = tasks.findIndex(item => item.id == oldTask.value.id);
        
        if (index !== -1) {
            tasks[index].name = taskName.value;
            tasks[index].time = taskTime.value;
        }
    }

    
    taskName.value = '';
    taskTime.value = '';
    isSidebarActive.value = false;
}
</script>

<template>
    <!-- component -->
    <div class="h-screen w-full flex items-center justify-center bg-teal-lightest font-sans">


        <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-xxl">

            <!-- ================= Add task button ================= -->
            <div class="mb-4">
                <h1 class="text-grey-darkest">Todo List</h1>
                <button @click="isSidebarActive = true"
                    class="w-full block p-2 border-2 rounded hover:border-green-500 hover:text-green-900 hover:bg-green-200">Add
                    Task</button>
            </div>

            <!-- ================= Task table ================= -->
            <div>
                <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
                    <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                            <tr>
                                <th scope="col" class="px-6 py-3">
                                    SL
                                </th>
                                <th scope="col" class="px-6 py-3">
                                    Task Name
                                </th>
                                <th scope="col" class="px-6 py-3">
                                    Time
                                </th>
                                <th scope="col" class="px-6 py-3 text-end">
                                    Action
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(task, index) in tasks" :key="index"
                                class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700">
                                <th class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
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
                                    <button @click="editTask(task.id)"
                                        class=" p-2 ml-2 border-2 rounded hover:border-yellow-500 hover:text-yellow-900 hover:bg-yellow-200">Edit</button>
                                    <button @click="removeTask(index)"
                                        class=" p-2 ml-2 border-2 rounded hover:border-red-500 hover:text-red-900 hover:bg-red-200">Remove</button>
                                </td>
                            </tr>

                        </tbody>
                    </table>
                </div>

            </div>
        </div>




        <!-- ================= drawer component ================= -->
        <div class="fixed  top-0 right-0 z-40 w-96 h-screen p-4 overflow-y-auto transition-transform bg-white"
            :class="isSidebarActive ? 'transform-none' : 'translate-x-full'">
            <h5 class="text-base font-semibold text-gray-500 uppercase">Menu </h5>

            <!-- Close button -->
            <button type="button" @click="isSidebarActive = false"
                class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 absolute top-2.5 end-2.5 inline-flex items-center">
                <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"
                    xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd"
                        d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                        clip-rule="evenodd"></path>
                </svg>
                <span class="sr-only">Close menu</span>
            </button>

            <!-- Task form -->
            <div class="py-4 overflow-y-auto border-t mt-5">
                <form @submit.prevent="addNewTask()">
                    <div class="mb-5">
                        <label for="task-name"> Task name </label>
                        <input type="text" id="task-name" v-model="taskName"
                            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
                            placeholder="Name here ...">
                        <p id="filled_error_help" v-show="errors[0]?.name"
                            class="mt-2 text-xs text-red-600 dark:text-red-400">{{ errors[0]?.name }}.</p>
                    </div>
                    <div class="mb-5">
                        <label for="time">time</label>
                        <input type="number" id="time" v-model="taskTime"
                            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
                            placeholder="Time here ...">
                        <p id="filled_error_help" v-show="errors[0]?.time"
                            class="mt-2 text-xs text-red-600 dark:text-red-400">{{ errors[0]?.time }}.</p>
                    </div>
                    <button
                        class="text-white bg-blue-700  hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center">Submit</button>
                </form>
            </div>


        </div>

        <!-- Transparent layer -->
        <div class="bg-gray-900/50 dark:bg-gray-900/80 fixed inset-0 z-30" @click="isSidebarActive = false" v-show="isSidebarActive"></div>
    </div>
</template>

<style scoped></style>