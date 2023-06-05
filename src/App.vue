<template>
  <div class="mainApp">
    <button class="mainApp--log">log</button>
    <div class="mainApp__clock">
      <p class="mainApp__clock--currentDate">{{ formattedDate }}</p>
      <p class="mainApp__clock--currentTime">{{ formattedTime }}</p>
    </div>
    <div class="todo">
      <div class="todo__text">Todo List</div>
      <div class="todo__items">
        <TodoItem
          v-for="item in 5"
          :key="item"
          :todoItem="{
            title: 'title',
            description: 'description',
            expireDate: new Date(),
            progress: 'beforeStart',
            category: [
              { name: 'category1', color: '#f3cc92' },
              { name: 'category2', color: '#f3ecee' },
              { name: 'category3', color: '#636063' },
            ],
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
import TodoItem from "./components/todoItem.vue";
const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

export default {
  name: "App",
  data() {
    return {
      currentDate: new Date(),
    };
  },
  components: {
    TodoItem,
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
};
</script>

<style>
.addTodo {
  cursor: pointer;
}
</style>
