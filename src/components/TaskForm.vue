<template>
  <Form
    ref="taskForm"
    :validation-schema="validationSchema"
    @submit="onSubmit"
    class="flex flex-col gap-2"
  >
    <label for="name">Name</label>
    <Field
      id="name"
      type="text"
      name="name"
      class="p-2 border-2 rounded-sm border-gray-300"
    />
    <ErrorMessage name="name" class="text-red-700" />
    <label for="description">Description</label>
    <Field
      id="description"
      type="text"
      name="description"
      class="p-2 border-2 rounded-sm border-gray-300"
    />
    <ErrorMessage name="description" class="text-red-700" />
    <label for="category">Category</label>
    <Field
      id="category"
      as="select"
      name="category"
      class="p-2 border-2 rounded-sm border-gray-200"
    >
      <option value="Design">Design</option>
      <option value="Development">Development</option>
      <option value="Testing">Testing</option>
      <option value="Deployment">Deployment</option>
      <option value="Building">Building</option>
      <option value="Research">Research</option>
    </Field>
    <ErrorMessage name="category" class="text-red-700" />
    <label for="status">Status</label>
    <Field
      id="status"
      as="select"
      name="status"
      class="p-2 border-2 rounded-sm border-gray-200"
    >
      <option value="To Do">To Do</option>
      <option value="In Progress">In Progress</option>
      <option value="Review">Review</option>
      <option value="Done">Done</option>
    </Field>
    <ErrorMessage name="status" class="text-red-700" />
    <button type="submit">Submit</button>
  </Form>
</template>
<script setup lang="ts">
import { onMounted, ref } from "vue";
import { toTypedSchema } from "@vee-validate/zod";
import { ErrorMessage, Field, Form } from "vee-validate";

import { taskFormSchema } from "@/schemas";
import kanbanStore from "@/stores/kanbanStore";
import { ACTIONS, type Column, type Task } from "@/types";

const emit = defineEmits<{
  (e: "close-modal"): void;
}>();

const props = defineProps<{
  columnId: Column["columnId"];
  task?: Task;
  action: ACTIONS;
}>();

const taskForm = ref();

onMounted(() => {
  if (props.action === ACTIONS.UPDATE_TASK) {
    taskForm.value.setFieldValue("name", props.task?.name);
    taskForm.value.setFieldValue("description", props.task?.description);
    taskForm.value.setFieldValue("category", props.task?.category);
    taskForm.value.setFieldValue("status", props.task?.status);

  }
});

let validationSchema = toTypedSchema(taskFormSchema);

function onSubmit(values: any) {
  if (props.action === ACTIONS.ADD_TASK) {
    kanbanStore.addTaskToColumn(props.columnId, {
      name: values.name,
      description: values.description,
      category: values.category,
      status: values.status,
    });
  } else if (props.action === ACTIONS.UPDATE_TASK && props.task) {
    let updatedTask = {
      taskId: props.task.taskId,
      name: values.name,
      description: values.description,
      category: values.category,
      status: values.status,
    };

    kanbanStore.updateTask(props.columnId, updatedTask);
  }
  emit("close-modal");
}
</script>
