<!-- TypeScript -->
<script setup lang="ts">
import { ref } from 'vue';

// Handling the light and dark themes.
const colorMode = useColorMode()
const isDark = computed({
  get () {
    return colorMode.value === 'light'
  },
  set () {
    colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
  }
});

const status = ref<string[]>([]);
const newStatus = ref('');

const addStatus = () => {
  if (newStatus.value.trim()) {
    status.value.push(newStatus.value.trim());
    newStatus.value = ''; // Clear input field after adding
  }
};

// Modal 
const isOpen = ref(false);
</script>

<!-- HTML & CSS -->
<template>
  <div class="wrapper max-w-[1200px] m-auto pb-24">
    <!-- Navbar -->
    <div class="nav flex justify-between max-w-[1200px] p-8">
      <h1 class="text-2xl font-mono">Notion Tasks.</h1>
      <!-- Toggle Theme -->
      <ClientOnly>
        <UButton
          :icon="isDark ? 'i-heroicons-moon-solid' : 'i-heroicons-sun-solid'"
          color="gray"
          variant="ghost"
          aria-label="Theme"
          @click="isDark = !isDark"
        />
      </ClientOnly>
    </div>  

    <!-- Description -->
    <div class="desc text-center m-4 p-8 text-xl font-mono bg-gradient-to-r from-blue-600 via-green-500 to-indigo-400 text-transparent bg-clip-text">
      <p>Notion Tasks helps in scheduling your tasks,</p>
      <p>Drag & Drop feature enables flexibility.</p>
    </div>

    <!-- input field -->
    <div class="flex justify-center mb-8">
      <UButton @click="isOpen = true" class="text-lg">Create <span class="text-3xl pb-1">+</span></UButton>
      <UModal v-model="isOpen">
        <div class="p-14 flex flex-col gap-8 text-lg justify-center m-4">
          <p class="text-center">Enter the status name & click on "submit"</p>
          <input v-model="newStatus" type="text" placeholder="Status name..." class="border rounded-md px-2 py-1 mr-2">
          <div class="btns flex justify-between">
            <button @click="isOpen = false; newStatus='' " class="text-lg px-2 py-1 rounded-md bg-blue-500 text-white hover:bg-blue-700">Discard</button>
            <button @click="addStatus();isOpen = false" class="text-lg px-2 py-1 rounded-md bg-blue-500 text-white hover:bg-blue-700">Submit</button>
          </div>
        </div>
      </UModal>
    </div>

    <!-- Status Grid -->
    <div class="status grid grid-cols-3 gap-8">
      <template v-for="item in status" :key="item">
        <Status :title="item"/>
      </template>
    </div>
  </div>
</template>
