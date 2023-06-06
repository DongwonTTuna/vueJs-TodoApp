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
                {{ category.name }}
              </div>
            </td>
          </tr>
          <tr class="filter__table__progress">
            <td class="filter__table--progress--text">Progress</td>
            <td class="filter__table--progress">
              <div
                @click="filterTodoByProgress(0, $event)"
                class="filter__table--progress--undone"
              >
                Undone
              </div>
              <div
                @click="filterTodoByProgress(1, $event)"
                class="filter__table--progress--finished"
              >
                Finished
              </div>
              <div
                @click="filterTodoByProgress(2, $event)"
                class="filter__table--progress--progressing"
              >
                Progressing
              </div>
              <div
                @click="filterTodoByProgress(3, $event)"
                class="filter__table--progress--beforeStart"
              >
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
      categories: [{ color: "blue", name: "General"},{ color: "orange", name: "Important"},{ color: "red", name: "Emergency"}],
      todoItems: [],
      backupedTodoItems: [],
    };
  },
  components: {
    TodoItem: TodoItemComponent,
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
      console.log("editTodo");
    },
    filterTodoByProgress(progress: number, event: Event) {
      const filteringFunction = (filterOpt: Progress, event: Event) => {
        this.todoItems = this.backupedTodoItems.filter((todoItem) => {
          todoItem!.progress === filterOpt;
        });
        const helem = event.target as HTMLElement;
        if (helem.classList.contains("--active")) {
          helem.classList.remove("--active");
        } else {
          helem.classList.add("--active");
        }
      };
      if (event === null) return;
      switch (progress) {
        case 0:
          return filteringFunction("Undone", event);
        case 1:
          return filteringFunction("Finished", event);
        case 2:
          return filteringFunction("Progressing", event);
        case 3:
          return filteringFunction("beforeStart", event);
      }
    },
  },
};
</script>

<style>
.addTodo {
  cursor: pointer;
}
</style>
