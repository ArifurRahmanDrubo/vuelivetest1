<template>
  <div class="p-4">
    <h1 class="text-3xl  font-semibold"> Task List</h1>
    <div v-for="(task, index) in tasks" :key="index" class="mb-4 p-2 bg-gray-100 rounded-md">
      <span class="mr-4">{{ task.name }} - {{ task.time }} minutes</span>
      <button @click="openEditModal(index)" class="bg-blue-500 text-white px-2 py-1 rounded">Edit</button>
    </div>

    <modal v-show="showModal" @submit="updateTask" @close="closeModal" class="fixed top-0 left-0 w-full h-full flex items-center justify-center">
      <form class="bg-white p-4 rounded-md shadow-md">
        <label for="editName" class="block mb-2">Name:</label>
        <input v-model="editedTask.name" type="text" id="editName" class="w-full p-2 mb-4 border border-gray-300 rounded-md" />

        <label for="editTime" class="block mb-2">Time:</label>
        <input v-model="editedTask.time" type="number" id="editTime" class="w-full p-2 mb-4 border border-gray-300 rounded-md" />

        <div class="flex justify-end">
          <button type="button" @click="closeModal" class="mr-2 bg-gray-500 text-white px-4 py-2 rounded-md">Cancel</button>
          <button type="button" @click.prevent="updateTask" class="bg-blue-500 text-white px-4 py-2 rounded-md">Submit</button>
        </div>
      </form>
    </modal>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// reactive variables
const tasks = ref([
  { name: 'Task 1', time: 30 },
  { name: 'Task 2', time: 40 },
  { name: 'Task 3', time: 60 },
  { name: 'Task 4', time: 45 },
  { name: 'Task 5', time: 50 },
]);
const showModal = ref(false);
const editedTask = ref({ name: '', time: 0 });
const editedTaskIndex = ref(-1);

// functions
function openEditModal(index) {
  editedTask.value = { ...tasks.value[index] };
  editedTaskIndex.value = index;
  showModal.value = true;
}
function updateTask() {
  if (editedTaskIndex.value !== -1) {
    tasks.value[editedTaskIndex.value].name = editedTask.value.name;
    tasks.value[editedTaskIndex.value].time = editedTask.value.time;

    // Close the modal
    closeModal();
  }
}
function closeModal() {
  showModal.value = false;
  editedTask.value = { name: '', time: 0 };
  editedTaskIndex.value = -1;
}
</script>

<style>
/* Style your modal and global styles here if needed */
</style>
