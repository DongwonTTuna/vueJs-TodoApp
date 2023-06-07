<template>
  <table class="todoItem">
    <tr>
      <td
        v-for="(categoryItem, index) in todoItem.category"
        class="todoItem__category"
        :style="{
          backgroundColor: categoryItem.color,
          zIndex: todoItem.category.length - index,
          left: index * 20 + 'px',
        }"
      ></td>
      <td class="todoItem__progress">
        <img
          @dblclick="changeProgress"
          alt="progressStatus"
          :src="
            todoItem.progress === 'Undone'
              ? '/images/todo/late.png'
              : todoItem.progress === 'Finished'
              ? '/images/todo/completed.png'
              : todoItem.progress === 'Progressing'
              ? '/images/todo/progressing.png'
              : '/images/todo/beforeStart.png'
          "
        />
      </td>
      <td class="todoItem__buttons">
        <button
          class="todoItem__buttons--remove"
          @click="removeTodoItem"
        >
          X
        </button>
      </td>
      <td class="todoItem__titleTD">
        <div
          class="todoItem__title"
          :style="{
            backgroundColor:
              todoItem.progress === 'Undone'
                ? '#817e82f0'
                : todoItem.progress === 'Finished'
                ? '#f3ecee'
                : todoItem.progress === 'Progressing'
                ? '#f3cc92'
                : 'aliceblue',
          }"
          @dblclick="editTitle"
        >
          <input
            v-if="editingTitle"
            v-model="editedTitle"
            @blur="saveTitle"
            ref="titleInput"
          />
          <span v-else>{{ todoItem.title }}</span>
        </div>
      </td>
      <td class="todoItem__description">
        <div @dblclick="editDescription" v-show="!editingDescription">
          {{ todoItem.description }}
        </div>
        <textarea
          v-if="editingDescription"
          v-model="editedDescription"
          @blur="saveDescription"
          ref="descriptionInput"
        ></textarea>
      </td>
      <td class="todoItem__expireTD">
        <div class="todoItem__expireDate" @dblclick="editExpireDate">
          {{
            `${todoItem.expireDate.getDate()}.${(
              todoItem.expireDate.getMonth() + 1
            )
              .toString()
              .padStart(2, "0")}.${todoItem.expireDate.getFullYear()} (${
              daysOfWeek[todoItem.expireDate.getDay()]
            })`
          }}
          <br />
          {{
            todoItem.expireDate.toLocaleString("en-US", {
              hour: "numeric",
              minute: "numeric",
              hour12: true,
            })
          }}
        </div>
        <input
          v-if="editingExpireDate"
          type="datetime-local"
          v-model="editedExpireDate"
          @blur="saveExpireDate"
          ref="expireDateInput"
        />
      </td>
    </tr>
  </table>
</template>

<script lang="ts">
import { PropType } from "vue";

type Progress = "Undone" | "Finished" | "Progressing" | "beforeStart";
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
export default {
  name: "TodoItem",
  data() {
    return {
      daysOfWeek: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"], // Define the daysOfWeek array
      editingTitle: false,
      editedTitle: "",
      editingDescription: false,
      editedDescription: "",
      editingExpireDate: false,
      editedExpireDate: "",
    };
  },
  props: {
    todoItem: {
      type: Object as PropType<TodoItem>,
      required: true,
    },
  },
  methods: {
    editTitle() {
      this.editingTitle = true;
      this.editedTitle = this.todoItem.title; 
      this.$nextTick(() => this.$refs.titleInput.focus());
    },
    saveTitle() {
      this.editingTitle = false;
      this.todoItem.title = this.editedTitle;
    },
    editDescription() {
      this.editingDescription = true;
      this.editedDescription = this.todoItem.description;
      this.$nextTick(() => this.$refs.descriptionInput.focus());
    },
    saveDescription() {
      this.editingDescription = false;
      this.todoItem.description = this.editedDescription;
    },
    editExpireDate() {
      this.editingExpireDate = true; 

      // Format the date and time string
      const formattedDate = this.formatDate(this.todoItem.expireDate);

      this.editedExpireDate = formattedDate;
      this.$nextTick(() => this.$refs.expireDateInput.focus());
    },

    saveExpireDate() {
      this.editingExpireDate = false;

      // Parse the edited date and time string
      const parsedDate = this.parseDate(this.editedExpireDate);

      this.todoItem.expireDate = parsedDate;
    },

    formatDate(date) {
      // Convert the date to the desired format: "YYYY-MM-DDTHH:mm"
      const year = date.getFullYear();
      const month = (date.getMonth() + 1).toString().padStart(2, "0");
      const day = date.getDate().toString().padStart(2, "0");
      const hours = date.getHours().toString().padStart(2, "0");
      const minutes = date.getMinutes().toString().padStart(2, "0");

      return `${year}-${month}-${day}T${hours}:${minutes}`;
    },

    parseDate(dateString) {
      // Parse the date string in the format: "YYYY-MM-DDTHH:mm"
      const parts = dateString.split("T");
      const dateParts = parts[0].split("-");
      const timeParts = parts[1].split(":");

      const year = parseInt(dateParts[0]);
      const month = parseInt(dateParts[1]) - 1;
      const day = parseInt(dateParts[2]);
      const hours = parseInt(timeParts[0]);
      const minutes = parseInt(timeParts[1]);

      return new Date(year, month, day, hours, minutes);
    },
    changeProgress() {
      if (this.todoItem.progress === "Undone") {
        this.todoItem.progress = "Progressing";
      } else if (this.todoItem.progress === "Progressing") {
        this.todoItem.progress = "Finished";
      } else if (this.todoItem.progress === "Finished") {
        this.todoItem.progress = "beforeStart";
      } else {
        this.todoItem.progress = "Undone";
      }
    },
    removeTodoItem(){
      this.$emit("removeTodoItem");
    },
  },
};
</script>
