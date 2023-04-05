<template>
  <div class="bg-gray-50 mx-auto px-5 py-3">
    <div class="text-center mb-0 rounded-0">
      <!-- Input field for Template name -->
      <div class="flex justify-between items-center">
        <input
          id="email"
          v-model="templateName"
          type="text"
          name="email"
          class="block rounded-md border-0 py-2 px-3 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 w-[100%]"
          placeholder="Enter Template Name"
        />
      </div>
    </div>
  </div>
  <!-- Input field for Template Subject -->
  <div class="mx-4 mt-4">
    <input
      id="email"
      v-model="templateSubject"
      type="text"
      name="email"
      class="block mb-3 px-3 rounded-md border-0 py-2 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 w-[100%]"
      placeholder="Enter Subject"
    />
    <!-- Textarea for template body -->
    <textarea
      v-model="templateBody"
      rows="4"
      class="p-4 h-[calc(100vh-350px)] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-300 sm:py-1.5 sm:text-sm sm:leading-6"
      placeholder="Add Template Body..."
    />
    <!-- Buttons for template  -->
    <div class="flex justify-end mr-3 mt-4">
      <button
        type="button"
        class="border rounded-md bg-white py-2 px-3 text-sm font-semibold text-gray-600 shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 mr-3"
      >
        Cancel
      </button>
      <button
        type="button"
        class="rounded-md bg-indigo-600 py-2 px-4 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        @click="addTemplate"
      >
        Send
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
// Import vue
import { ref } from "vue";

const emit = defineEmits(['add-template'])
const templates = [];
// Declaring variables
let templateBody = ref("");
let templateName = ref("");
let templateSubject = ref("");

// Add Template to the template data
const addTemplate = () => {
  emit('add-template',{
    name: templateName.value,
    body: templateBody.value,
    subject: templateSubject.value,
  });
  templateName.value = "";
  templateBody.value = "";
  templateSubject.value = "";
};

</script>
