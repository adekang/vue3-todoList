<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" v-model="isCheckAll" />
    </label>
    <span>
      <span>已完成{{ count }}</span>
      / 全部{{ todos.length }}
    </span>
    <button class="btn btn-danger" @click="clearTodo">清除已完成任务</button>
  </div>
</template>
<script lang="ts">
import { computed, defineComponent } from "vue";
import { Todo } from "../types/todo";
export default defineComponent({
  props: {
    todos: {
      type: Array as () => Todo[],
      required: true,
    },
    checkAll: {
      type: Function,
      required: true,
    },
    clearAlloption: {
      type: Function,
      required: true,
    },
  },
  setup(props) {
    const count = computed(() => {
      return props.todos.reduce((pre, todo, index) => pre + (todo.isCompleted ? 1 : 0), 0);
    });

    // 全选 全不选
    const isCheckAll = computed({
      get() {
        return count.value > 0 && props.todos.length === count.value;
      },
      set(val) {
        props.checkAll(val);
      },
    });
    const clearTodo = () => {
      props.clearAlloption();
    };
    return { count, isCheckAll, clearTodo };
  },
});
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}
.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
