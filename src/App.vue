<template>
  <TodoList />
</template>
<script lang="ts">
import { defineComponent } from "vue";
import TodoList from "./views/TodoList.vue";
export default defineComponent({
  name: "App",
  components: {
    TodoList,
  },
});
</script>
