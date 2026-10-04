<script setup="ts">
import { ref } from "vue";

const currentYear = ref(new Date().getFullYear());
</script>

<template>
  <footer class="bg-gray-800 text-white py-4 italic text-sm font-light">
    <div class="container mx-auto text-center">
      <p>&copy; {{ currentYear }} Kanban Board Built by ArinzeKings</p>
    </div>
  </footer>
</template>