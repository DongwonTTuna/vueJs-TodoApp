<template>
  <div class="mainApp">
    <button @click="goGoogle" class="mainApp--log">google</button>
    <div class="mainApp__clock">
      <p class="mainApp__clock--currentDate">{{ formattedDate }}</p>
      <p class="mainApp__clock--currentTime">{{ formattedTime }}</p>
    </div>
    <div class="todo">
      <div class="todo__text">Todo List</div>
      <div class="todo__filter">
        <div class="todo__filter--text">filtering</div>
        <table class="filter__table">
          <tr class="filter__table__progress">
            <td class="filter__table--progress--text">Progress</td>
            <td class="filter__table--progress">
              <button
                @click="currentProgress = 'Undone'"
                :class="
                  currentProgress === 'Undone'
                    ? 'filter__table--progress--undone--active'
                    : 'filter__table--progress--undone'
                "
              >
                Undone
              </button>
              <button
                @click="currentProgress = 'Finished'"
                :class="
                  currentProgress === 'Finished'
                    ? 'filter__table--progress--finished--active'
                    : 'filter__table--progress--finished'
                "
              >
                Finished
              </button>
              <button
                @click="currentProgress = 'Progressing'"
                :class="
                  currentProgress === 'Progressing'
                    ? 'filter__table--progress--progressing--active'
                    : 'filter__table--progress--progressing'
                "
              >
                Progressing
              </button>
              <button
                @click="currentProgress = 'beforeStart'"
                :class="
                  currentProgress === 'beforeStart'
                    ? 'filter__table--progress--beforeStart--active'
                    : 'filter__table--progress--beforeStart'
                "
              >
                BeforeStart
              </button>
              <button
                @click="currentProgress = 'All'"
                :class="
                  currentProgress === 'All'
                    ? 'filter__table--progress--All--active'
                    : 'filter__table--progress--All'
                "
              >
                All
              </button>
            </td>
          </tr>
        </table>
      </div>

      <div class="todo__items">
        <TodoItem
          v-for="item in filteredTodoItems"
          :key="item"
          :todoItem="item"
          @removeTodoItem="removeTodoItem(item.id)"
        />
        <div
          @click="addNewTodoItem"
          :style="{ backgroundImage: 'url(' + imageUrl + ')' }"
          class="image-container"
        ></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import TodoItemComponent from "./components/todoItem.vue";

type Progress = "Undone" | "Finished" | "Progressing" | "beforeStart" | "All";
interface Category {
  color: string;
  name: string;
}
interface TodoItem {
  id: number;
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
      currentProgress: "All",
      imageUrl: "/images/todo/addNew.png",
      currentDate: new Date(),
      categories: [
        { color: "blue", name: "General" },
        { color: "orange", name: "Important" },
        { color: "red", name: "Emergency" },
      ],
      todoItems: [
        {
          id: 0,
          title: "title",
          description: "description",
          expireDate: new Date(),
          progress: "beforeStart",
          category: this.categories,
        },
        {
          id: 1,
          title: "title",
          description: "description",
          expireDate: new Date(),
          progress: "Progressing",
          category: this.categories,
        },
        {
          id: 3,
          title: "title",
          description: "description",
          expireDate: new Date(),
          progress: "beforeStart",
          category: this.categories,
        },
        {
          id: 4,
          title: "title",
          description: "description",
          expireDate: new Date(),
          progress: "Finished",
          category: this.categories,
        },
        {
          id: 5,
          title: "title",
          description: "description",
          expireDate: new Date(),
          progress: "Undone",
          category: this.categories,
        },
      ],
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
    filteredTodoItems() {
      return this.currentProgress === "All"
        ? this.todoItems
        : this.todoItems.filter(
            (item) => item.progress === this.currentProgress
          );
    },
  },
  mounted() {
    setInterval(() => {
      this.currentDate = new Date();
    }, 1000);
  },
  methods: {
    addNewTodoItem() {
      const id = Math.max(...this.todoItems.map((item) => item.id)) + 1;

      this.todoItems.push({
        id: id,
        title: "title",
        description: "description",
        expireDate: new Date(),
        progress: this.currentProgress,
        category: { name: "General", color: "blue" },
      });
    },
    goGoogle() {
      window.location.href = "http://google.com";
    },
    removeTodoItem(todoItemId: number) {
      this.todoItems = this.todoItems.filter((todoItem) => {
        return todoItem.id !== todoItemId;
      });
    },
  },
};
</script>

<style>
.addTodo {
  cursor: pointer;
}
</style>
