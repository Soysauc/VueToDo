<template>
  <div class="app">
    <header class="app__header">
      <div class="app__header-row">
        <h1 class="app__title">ToDo's</h1>
        <EllipseS @toggle="toggleQuote" />
      </div>
      <div v-if="showQuote" class="quote-bubble">
        <span>This ellipse wasn't specified in the Figma</span>
      </div>
      <div class="app__header-toggle">
        <span
          :class="{ 'selected': true, 'active': view === 'open' }"
          @click="setView('open')"
        >
          Open
        </span>
        <span
          :class="{ 'selected': true, 'active': view === 'closed' }"
          @click="setView('closed')"
        >
          Closed
        </span>
      </div>
      <div class="app__gradient"></div>
    </header>
    <div class="app__content">
      <section class="app__scroller">
        <ToDos
          v-if="view === 'open'"
          :todos="openTodos"
          type="open"
          :markAsCompleted="markAsCompleted"
          :markAsOpen="markAsOpen"
          :toggleEditForm="toggleEditForm"
        />

        <ToDos
          v-else
          :todos="closedTodos"
          type="closed"
          @markAsCompleted="markAsCompleted"
          @markAsOpen="markAsOpen"
          @toggleEditForm="toggleEditForm"
        />
      </section>
      <AddNew :showForm="showForm || showEditForm" @toggleForm="toggleForm" />
    </div>
    <NewTodoForm v-if="showForm" @toggleForm="toggleForm" @addTodo="addTodo" />
  </div>
</template>

<script>
import EllipseS from './Ellipse.vue';
import AddNew from './AddNew.vue';
import NewTodoForm from './NewTodoForm.vue';
import {fetchTodos} from  "../API/fetchTodos" ;
import ToDos from "./Todos.vue"
export default {
  name: 'App',

  components: {
    EllipseS,
    ToDos,
    AddNew,
    NewTodoForm,
  },

  data() {
    return {
      todos: [],
      showForm: false,
      showQuote: false,
      view: 'open',
      showEditForm: false,
    };
  },

  computed: {
    openTodos() {
      return this.todos.filter(todo => !todo.completed);
    },

    closedTodos() {
      return this.todos.filter(todo => todo.completed);
    }
  },

  methods: {
    addTodo(newTodoItem) {
      this.todos = [newTodoItem, ...this.todos];
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },

    //ehh this is different
    setView(viewType) {
  this.view = viewType;
   console.log('Current View:', this.view);
  console.log('Open Todos:', this.openTodos);
  console.log('Closed Todos:', this.closedTodos);
     },


    markAsCompleted(id) {
      this.todos = this.todos.map(todo => (todo.id === id ? { ...todo, completed: true } : todo));
    },

    markAsOpen(id) {
      this.todos = this.todos.map(todo => (todo.id === id ? { ...todo, completed: false } : todo));
    },

    toggleQuote() {
      this.showQuote = !this.showQuote;
    },

    toggleForm() {
      this.showForm = !this.showForm;
    },

    toggleEditForm() {
      this.showEditForm = !this.showEditForm;
    }
  },

  mounted() {
    fetchTodos()
      .then(data => {
        this.todos = data;
        localStorage.setItem('todos', JSON.stringify(data));
      })
      .catch(error => {
        console.error('Error fetching todos:', error);
      });
  }
};
</script>

<style scoped>
@import '../assets/App.css';
</style>
