<template>
  <div class="flex justify-between px-4 mb-3 text-white py-4">
    <h1 class="font-bold text-3xl">Kanban Board</h1>
    <button class="flex items-center gap-2 px-4 py-2 bg-blue-600 text-white rounded" @click="isColumnModalActive = !isColumnModalActive">
      <PlusIcon class="w-5 h-5" />
      <span>Add Column</span>
  </button>
  </div>
  <Modal
    :is-modal-active="isColumnModalActive"
    :heading="`${ACTIONS.ADD_COLUMN.split('_').join(' ')}`"
    @close-modal="isColumnModalActive = !isColumnModalActive"
  >
    <ColumnForm
      :action="ACTIONS.ADD_COLUMN"
      @close-modal="isColumnModalActive = !isColumnModalActive"
    />
  </Modal>
</template>
<script setup lang="ts">
import { ref } from "vue";
import ColumnForm from "./ColumnForm.vue";
import Modal from "./common/Modal.vue";
import { ACTIONS } from "@/types";
import { PlusIcon } from "@heroicons/vue/24/outline";

const isColumnModalActive = ref(false);
</script>
