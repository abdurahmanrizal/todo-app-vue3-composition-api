<template>
  <div class="container" style="margin-top: 20px">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <Input v-model="todo" @add="add" />
          <div class="col-1">
            <button class="btn btn-success text-white shadow" @click="add">
              ADD +
            </button>
          </div>
        </div>
        <Search @searchTodo="searchTodo" />
        <List :todos="results" @doneTodo="doneTodo" @deleteTodo="deleteTodo" />
        <div class="d-flex mt-3">
          <p>
            <span class="text-primary fw-bold">Total: {{ totalTodo }}</span>
            <span class="fw-bold mx-2">
              Active:
              {{ parseInt(totalTodo) - parseInt(totalTodoDone) }}
            </span>
            <span class="text-success fw-bold mx-2"
              >Done : {{ totalTodoDone }}</span
            >
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, onMounted, reactive, ref } from "vue";
import List from "./components/List.vue";
import Search from "./components/Search.vue";
import Input from "./components/Input.vue";
export default {
  components: { List, Search, Input },
  setup() {
    const todo = ref("");
    const searchValue = ref("");
    const todos = reactive({
      lists: [],
    });

    onMounted(() => {
      todos.lists = localStorage.getItem("todos")
        ? JSON.parse(localStorage.getItem("todos"))
        : [];
    });

    const results = computed(() =>
      searchByValue(todos.lists, searchValue.value)
    );
    const totalTodo = computed(
      () => searchByValue(todos.lists, searchValue.value).length
    );
    const totalTodoDone = computed(
      () => searchByValue(todos.lists, searchValue.value, 1).length
    );
    // method untuk melakukan penambahan TODO
    const add = () => {
      todos.lists.unshift({
        activity: todo.value,
        isDone: false,
      });
      setLocalStorage(todos.lists);
      todo.value = "";
    };
    // method untuk melakukan aksi TODO
    const doneTodo = (indexTodo) => {
      todos.lists.filter((todo, index) => {
        if (index === indexTodo) {
          todo.isDone = !todo.isDone;
        }
      });
      setLocalStorage(todos.lists);
    };

    // method untuk menghapus TODO
    const deleteTodo = (indexTodo) => {
      todos.lists = todos.lists.filter((todo, index) => {
        if (index !== indexTodo) {
          return todo;
        }
      });
      setLocalStorage(todos.lists);
    };

    // method untuk melakukan pencarian data
    const searchTodo = (search) => {
      searchValue.value = search;
    };

    function setLocalStorage(lists) {
      return localStorage.setItem("todos", JSON.stringify(lists));
    }

    function searchByValue(dataTodo, search, done = 0) {
      return dataTodo.filter((todo) => {
        if (done) {
          return (
            todo.isDone &&
            (todo.activity.toLowerCase().includes(search) ||
              todo.activity.toUpperCase().includes(search))
          );
        } else {
          return (
            todo.activity.toLowerCase().includes(search) ||
            todo.activity.toUpperCase().includes(search)
          );
        }
      });
    }

    return {
      todo,
      todos,
      add,
      doneTodo,
      deleteTodo,
      searchValue,
      searchTodo,
      results,
      totalTodoDone,
      totalTodo,
    };
  },
};
</script>

<style></style>
