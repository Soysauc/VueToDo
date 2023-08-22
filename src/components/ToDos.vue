<template>
  <div>
    <template v-if="!editingTodo && !showForm">
      <SearchBar @search="handleSearch" />
      <div class="app__divider"></div>
      <div
        v-for="todo in filteredTodos"
        :key="todo.id"
        :class="['todos', `todos--${type}`]"
      >
        <div style="display: flex">
          <div v-if="todo.completed" class="tooltip-container">
            <svg
              class="todos__check"
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="14"
              viewBox="0 0 20 14"
              fill="none"
              @click="markAsOpen(todo.id)"
            >
              <mask
                id="mask0_2_479"
                :style="{ maskType: 'luminance' }"
                maskUnits="userSpaceOnUse"
                x="0"
                y="0"
                width="20"
                height="14"
              >
                <path
                  fillRule="evenodd"
                  clipRule="evenodd"
                  d="M0 0H20V13.4548H0V0Z"
                  fill="white"
                />
              </mask>
              <g mask="url(#mask0_2_479)">
                <path
                  fillRule="evenodd"
                  clipRule="evenodd"
                  d="M19.68 0.974474L19.0249 0.31935C18.8191 0.113334 18.5446 0 18.2516 0C17.9586 0 17.6839 0.113334 17.4781 0.31935L8.2727 9.52457L2.52131 3.77302C2.31545 3.56732 2.04082 3.45383 1.74781 3.45383C1.45496 3.45383 1.18049 3.56732 0.974474 3.77302L0.319188 4.42798C0.113171 4.63432 0 4.90912 0 5.2018C0 5.49465 0.113171 5.76928 0.319188 5.9753L6.81286 12.4686C6.82148 12.4809 6.83058 12.4922 6.84115 12.5026L7.49644 13.147C7.70229 13.3522 7.97693 13.4549 8.27221 13.4549H8.27563C8.5688 13.4549 8.84343 13.3522 9.04896 13.147L9.70441 12.4969C9.71498 12.4865 9.72392 12.4779 9.72929 12.4692L19.6799 2.51952C20.1067 2.09366 20.1067 1.40082 19.68 0.974474Z"
                  fill="white"
                />
              </g>
            </svg>
            <span class="tooltip-text">Un-Check?</span>
          </div>
          <svg
            v-else
            class="todos__circle"
            xmlns="http://www.w3.org/2000/svg"
            width="44"
            height="44"
            viewBox="0 0 44 44"
            fill="#3B3753"
          >
            <circle cx="22" cy="22" r="22" fill="#3B3753" />
          </svg>
          <div class="todos__content">
            <span class="todo__item-text">{{ todo.title }}</span>
            <div class="todo__item-id">User:{{ todo.userId }}</div>
          </div>
        </div>
        <div
          style="margin-right: 35px"
          @mouseenter="setQuote(todo.id, true)"
          @mouseleave="setQuote(todo.id, false)"
        >
          <EllipseS v-if="type === 'open'" :id="todo.id" />
          <div v-if="quoteId === todo.id" class="quote-bubble">
            <div class="quote-bubble__svg">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="8"
                height="6"
                viewBox="0 0 8 6"
                fill="none"
              >
                <path
                  d="M4 0L7.4641 6H0.535898L4 0Z"
                  fill="#070417"
                  fillOpacity="0.71"
                />
              </svg>
            </div>
            <span @click="startEditing(todo)">Edit ToDo</span>
            <div class="quote__divider"></div>
            <span @click="markAsCompleted(todo.id)">Mark Completed</span>
          </div>
        </div>
      </div>
    </template>
    <EditItem
      v-else
      :currentTodo="editingTodo"
      :editTodo="editTodo"
      @toggleForm="stopEditing"
    />
  </div>
</template>

<script>
import SearchBar from "./SearchBar";
import EllipseS from "./Ellipse";
import EditItem from "./EditItem";

export default {
  name: "ToDos",
  components: { SearchBar, EllipseS, EditItem },
  props: [
    "todos",
    "type",
    "addTodo",
    "markAsCompleted",
    "markAsOpen",
    "toggleEditForm",
  ],
  data() {
    return {
      filteredTodos: [],
      quoteId: null,
      editingTodo: null,
      showForm: false,
    };
  },
  watch: {
    todos() {
      this.filterTodos();
    },
    type() {
      this.filterTodos();
    },
  },
  methods: {
    startEditing(todo) {
      this.editingTodo = todo;
      this.toggleEditForm();
    },
    stopEditing() {
      this.editingTodo = null;
      this.toggleEditForm();
    },
    filterTodos() {
      this.filteredTodos = this.todos.filter((todo) =>
        this.type === "open" ? !todo.completed : todo.completed
      );
    },
    handleSearch(searchInput) {
      const filtered = this.todos.filter((todo) =>
        todo.title.toLowerCase().includes(searchInput.toLowerCase())
      );
      this.filteredTodos = filtered;
    },
    setQuote(id, state) {
      this.quoteId = state ? id : null;
    },
  },
};
</script>

<style scoped>
@import '../assets/Todos.css';
@import '../assets/App.css';
</style>
