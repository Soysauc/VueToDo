<template>
  <div class="new-todo-form">
    <div class="new-todo-form__content">
      <svg
        class="new-todo-form__arrow"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        @click="handleBackButton"
      >
        <path
          d="M11.4375 18.75L4.6875 12L11.4375 5.25"
          stroke="white"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M5.625 12L19.3125 12"
          stroke="white"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      <h2 class="new-todo-form__title">Edit ToDo</h2>
    </div>
    <input
      class="new-todo-form__input"
      type="text"
      placeholder="New Todo"
      v-model="editedTodo"
    />
    <select class="new-todo-form__select" v-model="selectedUser">
      <option value="">Select User</option>
      <option value="User1">User 1</option>
      <option value="User2">User 2</option>
      <option value="User3">User 3</option>
    </select>
    <select class="new-todo-form__select" v-model="selectedStatus">
      <option value="">Status</option>
      <option value="Completed">Completed</option>
      <option value="Incomplete">Incomplete</option>
    </select>
    <button class="new-todo-form__finish" @click="handleEditTodo">
      Finish
    </button>
    <button class="new-todo-form__quit" @click="handleBackButton">Quit</button>
  </div>
</template>

<script>
export default {
  name: "EditItem",
  props: {
    toggleForm: Function,
    editTodo: Function,
    currentTodo: Object,
  },
  data() {
    return {
      editedTodo: "",
      selectedUser: "",
      selectedStatus: "",
    };
  },
  watch: {
    currentTodo(newTodo) {
      if (newTodo) {
        this.editedTodo = newTodo.title;
        this.selectedUser = `User${newTodo.userId}`;
        this.selectedStatus = newTodo.completed ? "Completed" : "Incomplete";
      }
    },
  },
  methods: {
    handleEditTodo() {
      if (!this.editedTodo || !this.selectedUser || !this.selectedStatus) {
        alert("Please fill in all fields.");
        return;
      }

      const updatedTodoItem = {
        id: this.currentTodo ? this.currentTodo.id : Date.now(),
        title: this.editedTodo,
        userId: this.selectedUser.replace("User", ""),
        completed: this.selectedStatus === "Completed" ? true : false,
      };

      this.editTodo(updatedTodoItem, !!this.currentTodo);
      this.editedTodo = "";
      this.selectedUser = "";
      this.selectedStatus = "";
      this.toggleForm();
    },
    handleBackButton() {
      this.toggleForm();
    },
  },
};
</script>

<style scoped src="./styles/EditItem.css"></style>
