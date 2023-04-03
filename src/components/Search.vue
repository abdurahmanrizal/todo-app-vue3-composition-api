<template>
  <div class="row mt-3 justify-content-end">
    <div class="col-3">
      <input
        type="text"
        v-model="search"
        class="form-control"
        placeholder="Search TODO..."
        @keyup="searchTodo" />
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup(props, { emit }) {
    const search = ref("");
    const searchTodo = () => {
      emit("searchTodo", search.value);
    };
    return { search, searchTodo };
  },
};
</script>

<style></style>
