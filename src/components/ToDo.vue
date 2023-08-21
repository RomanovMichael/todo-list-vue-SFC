<template>
  <div class="wrapper">
    <section class="todo">
      <div class="todo__container">
        <div class="todo__content">
          <div class="todo__head">
            <h1 class="todo__title">TODO List</h1>
          </div>
          <div class="todo__form" v-on:submit.prevent="addTodo()">
            <form method="post" class="todo-form" id="todoForm">
              <div class="todo-form__group">
                <input
                  v-model="formInputValue"
                  type="text"
                  class="todo-form__input"
                  id="todoInput"
                  placeholder="What to do..."
                />
                <button type="submit" class="todo-form__btn">Add Task</button>
              </div>
            </form>
          </div>
          <div class="todo__search" id="todoSearch" v-if="todoList.length > 0">
            <div class="todo__search-input-wrap">
              <div class="todo__search-icon">
                <svg
                  enable-background="new 0 0 32 32"
                  viewBox="0 0 32 32"
                  xml:space="preserve"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                >
                  <path
                    d="M27.414,24.586l-5.077-5.077C23.386,17.928,24,16.035,24,14c0-5.514-4.486-10-10-10S4,8.486,4,14  s4.486,10,10,10c2.035,0,3.928-0.614,5.509-1.663l5.077,5.077c0.78,0.781,2.048,0.781,2.828,0  C28.195,26.633,28.195,25.367,27.414,24.586z M7,14c0-3.86,3.14-7,7-7s7,3.14,7,7s-3.14,7-7,7S7,17.86,7,14z"
                    id="XMLID_223_"
                  />
                </svg>
              </div>
              <input
                v-model="searchInputValue"
                type="text"
                class="todo__search-input"
                id="todoSearchInput"
                placeholder="Search"
              />
            </div>
            <div
              class="todo__notfound"
              v-if="searchInputValue !== '' && getTodoList.length == 0"
            >
              Nothing found
            </div>
          </div>
          <div class="todo__list" id="todoList">
            <div
              class="todo__item-wrap"
              @click="editTodo(index)"
              v-for="(item, index) in getTodoList"
              :key="item.id"
            >
              <div class="todo__item">
                <div class="todo__item-text" v-if="editTodoIndex !== index">
                  {{ item.title }}
                </div>
                <input
                  type="text"
                  class="todo__item-edit-input"
                  v-model="item.title"
                  v-if="editTodoIndex == index"
                />
                <button
                  class="todo__item-delete"
                  v-if="editTodoIndex !== index"
                  @click.stop="deleteTodo(index)"
                >
                  <span class="todo__item-delete-thumb"></span>
                </button>
                <button
                  class="todo__item-save"
                  v-if="editTodoIndex == index"
                  @click.stop="saveTodo"
                >
                  Save
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "ToDo",
  data() {
    return {
      APP_KEY: "app_todo",
      todoList: [],
      editTodoIndex: null,
      formInputValue: "",
      searchInputValue: "",
    };
  },
  computed: {
    getTodoList() {
      if (this.searchInputValue === "") {
        return this.todoList;
      } else {
        return this.todoList.filter((element) =>
          element.title.includes(this.searchInputValue)
        );
      }
    },
  },
  methods: {
    addTodo() {
      if (this.formInputValue === "") {
        return;
      }

      this.todoList.push({ title: this.formInputValue, id: toString(new Date())});
      this.formInputValue = "";
      localStorage.setItem(this.APP_KEY, JSON.stringify(this.todoList));
    },
    deleteTodo(index) {
      this.todoList.splice(index, 1);
      localStorage.setItem(this.APP_KEY, JSON.stringify(this.todoList));
    },
    editTodo(index) {
      this.editTodoIndex = index;
    },
    saveTodo() {
      this.editTodoIndex = null;
      localStorage.setItem(this.APP_KEY, JSON.stringify(this.todoList));
    },
  },
  mounted() {
    this.todoList = JSON.parse(localStorage.getItem(this.APP_KEY)) || [];
  },
};
</script>

