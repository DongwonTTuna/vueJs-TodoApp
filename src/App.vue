<template>
  <div class="mainApp">
    <button class="mainApp--log">log</button>
    <div class="mainApp__clock">
      <p class="mainApp__clock--currentDate">{{ formattedDate }}</p>
      <p class="mainApp__clock--currentTime">{{ formattedTime }}</p>
    </div>
    <div class="todo">
      <div class="todo__text">Todo List</div>
      <div class="todo__filter">
        <div class="todo__filter--text">filtering</div>
        <table class="filter__table">
          <tr class="filter__table--category">
            <td class="filter__table--category--text">Category</td>
            <td class="filter__table--category">
              <div
                v-for="category in categories"
                class="filter__table--category--item"
              >
                {{ category }}
              </div>
            </td>
          </tr>
          <tr class="filter__table__progress">
            <td class="filter__table--progress--text">Progress</td>
            <td class="filter__table--progress">
              <div @click="filterTodoByProgress(0)" class="filter__table--progress--undone">Undone</div>
              <div @click="filterTodoByProgress(1)" class="filter__table--progress--finished">Finished</div>
              <div @click="filterTodoByProgress(2)" class="filter__table--progress--progressing">
                Progressing
              </div>
              <div @click="filterTodoByProgress(3)" class="filter__table--progress--beforeStart">
                BeforeStart
              </div>
            </td>
          </tr>
        </table>
      </div>

      <div class="todo__items">
        <TodoItem
          v-for="item in 5"
          :key="item"
          :todoItem="{
            title: 'title',
            description: 'description',
            expireDate: new Date(),
            progress: 'beforeStart',
            category: categories,
          }"
        />
        <img
          class="addTodo"
          loading="lazy"
          src="/images/todo/addNew.png"
          alt="addItem"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">

import TodoItemComponent from "./components/todoItem.vue";


type Progress = "Undone" | "Finished" | "Progressing" | "beforeStart";
interface Category {
  color: string;
  name: string;
}
interface TodoItem {
  title: string;
  description: string;
  category: Array<Category>;
  progress: Progress;
  expireDate: Date;
}
const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

export default {
  name: "App",
  data() {
    return {
      currentDate: new Date(),
      categories: [],
      todoItems: []
    };
  },
  components: {
    "TodoItem" : TodoItemComponent,
  },
  computed: {
    formattedDate() {
      const { currentDate } = this;
      return `${currentDate.getDate()}.${(currentDate.getMonth() + 1)
        .toString()
        .padStart(2, "0")}.${currentDate.getFullYear()} (${
        daysOfWeek[currentDate.getDay()]
      })`;
    },
    formattedTime() {
      const { currentDate } = this;
      return currentDate.toLocaleString("en-US", {
        hour: "numeric",
        minute: "numeric",
        hour12: true,
      });
    },
  },
  mounted() {
    setInterval(() => {
      this.currentDate = new Date();
    }, 1000);
  },
  methods: {
    addTodo(todoIndex: number, todoItem: TodoItem) {
      console.log("addTodo");
    },
    removeTodo(todoIndex: number, todoItem: TodoItem) {
      console.log("removeTodo");
    },
    editTodo(todoIndex: number, todoItem: TodoItem) {
      console.log("editTodo")
    },
    filterTodoByProgress(progress: number){
      
      switch(progress){
        case 0: return console.log("Undone");
        case 1: return console.log("Finished");
        case 2: return console.log("Progressing");
        case 3: return console.log("beforeStart");
      }

    }
  }
};
</script>

<style>
.addTodo {
  cursor: pointer;
}
</style>
