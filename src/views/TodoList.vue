<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo="addTodo" />
      <List :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo" />
      <Footer :todos="todos" :checkAll="checkAll" :clearAlloption="clearAlloption" />
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, reactive, toRefs, watch, onMounted, provide } from "vue";
import Header from "../components/header.vue";
import List from "../components/list.vue";
import Footer from "../components/footer.vue";
import { Todo } from "../types/todo";
import { saveTodos, readTodos } from "../utils/localStorageUtils";

export default defineComponent({
  name: "TodoList",
  components: {
    Header,
    List,
    Footer,
  },
  setup() {
    const state = reactive<{ todos: Todo[] }>({
      todos: [],
    });
    onMounted(() => {
      setTimeout(() => {
        state.todos = readTodos();
      }, 1000);
    });
    const addTodo = (todo: Todo) => {
      state.todos.unshift(todo);
    };
    const deleteTodo = (index: number) => {
      state.todos.splice(index, 1);
    };
    //  修改todo 的 iscompleted 状态
    const updateTodo = (todo: Todo, iscompleted: boolean) => {
      todo.isCompleted = iscompleted;
    };

    provide("deleteTodo", deleteTodo);
    provide("updateTodo", updateTodo);

    // 全选全不选的方法
    const checkAll = (iscompleted: boolean) => {
      state.todos.forEach(todo => {
        todo.isCompleted = iscompleted;
      });
    };
    //  删除所有选中数据
    const clearAlloption = () => {
      state.todos = state.todos.filter(todo => !todo.isCompleted);
    };

    //  存储在浏览器里面
    // watch(
    //   () => state.todos,
    //   value => {
    //     saveTodo(value);
    //   },
    //   { deep: true }
    // );
    watch(() => state.todos, saveTodos, { deep: true });
    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAll,
      clearAlloption,
    };
  },
});
</script>

<style scoped>
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
