<template>
  <li
    @mouseenter="mouseHandler(true)"
    @mouseleave="mouseHandler(false)"
    :style="{ backgroundColor: bgcolor, color: mycolor }"
  >
    <label>
      <input type="checkbox" v-model="isCompleted" />
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger" @click="delTodo" v-show="isShow">删除</button>
  </li>
</template>
<script lang="ts">
import { defineComponent, ref, computed, inject } from "vue";
import { Todo } from "../types/todo";
export default defineComponent({
  name: "item",
  props: {
    todo: {
      type: Object as () => Todo,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    updateTodo: {
      type: Function,
      required: true,
    },
  },
  setup(props) {
    const bgcolor = ref("white");
    const mycolor = ref("black");
    const isShow = ref(false);
    const mouseHandler = (flag: boolean) => {
      if (flag) {
        bgcolor.value = "pink";
        mycolor.value = "green";
        isShow.value = !isShow.value;
      } else {
        bgcolor.value = "white";
        mycolor.value = "black";
        isShow.value = !isShow.value;
      }
    };
    const deletItem: Function | undefined = inject("deleteTodo");
    const updateItem: Function | undefined = inject("updateTodo");

    const delTodo = () => {
      if (window.confirm("确定要删除吗？")) {
        if (deletItem != undefined) {
          deletItem(props.index);
        }
      }
    };

    const isCompleted = computed({
      get() {
        return props.todo.isCompleted;
      },
      set(val) {
        if (updateItem != undefined) {
          updateItem(props.todo, val);
        }
      },
    });
    return { mouseHandler, bgcolor, mycolor, isShow, delTodo, isCompleted };
  },
});
</script>

<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  margin-top: 3px;
}
li:before {
  content: initial;
}
li::last-child {
  border-bottom: none;
}
</style>
