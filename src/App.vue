<template>
  <div id="app">
    <todo-component></todo-component>
  </div>
</template>
<script>
import TodoComponent from "./components/todo/TodoComponent.vue";

export default {
  name: "App",
  components: {
    TodoComponent,
  },
};
</script>
<style></style>
