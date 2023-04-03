<template>
  <div class="mt-4">
    <ul class="list-group" v-if="todos.length > 0">
      <li class="list-group-item" v-for="(todo, index) in todos" :key="index">
        <div class="row">
          <div class="col">
            <span
              :class="[
                { 'text-success': todo.isDone },
                { 'line-through': todo.isDone },
              ]"
              >{{ todo.activity.toUpperCase() }}</span
            >
          </div>
          <div class="col-auto">
            <div class="row gx-2">
              <div class="col">
                <button
                  class="btn btn-primary text-white shadow"
                  @click="doneTodo(index)"
                  v-if="!todo.isDone">
                  DONE
                </button>
                <button
                  class="btn btn-warning text-white shadow"
                  @click="doneTodo(index)"
                  v-else>
                  UNDONE
                </button>
              </div>
              <div class="col">
                <button
                  class="btn btn-danger shadow"
                  @click="deleteTodo(index)">
                  x
                </button>
              </div>
            </div>
          </div>
        </div>
      </li>
    </ul>
    <p class="text-center fw-bold" v-else>Data not found...</p>
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  setup(props, { emit }) {
    const doneTodo = (index) => {
      emit("doneTodo", index);
    };
    const deleteTodo = (index) => {
      emit("deleteTodo", index);
    };
    return { doneTodo, deleteTodo };
  },
};
</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>
