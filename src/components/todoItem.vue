<template>
  <table class="todoItem">
    <tr>
      <td
        v-for="(categoryItem, index) in todoItem.category"
        class="todoItem__category"
        :style="{
          backgroundColor: categoryItem.color,
          zIndex: todoItem.category.length - index,
          left: index * 20 + 'px'
        }"
      ></td>
      <td class="todoItem__progress">
        <img
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
        >
          {{ todoItem.title }}
        </div>
      </td>
      <td class="todoItem__description">
        {{ todoItem.description }}
      </td>
      <td class="todoItem__expireTD">
        <div class="todoItem__expireDate">
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
      </td>
    </tr>
  </table>
</template>

<style>
/* CSSスタイルを追加 */
</style>

<script lang="ts">
import { PropType } from "vue";

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
export default {
  name: "TodoItem",
  data() {
    return {
      daysOfWeek: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
    };
  },
  props: {
    todoItem: {
      type: Object as PropType<TodoItem>,
      required: true,
    },
  },
};
</script>
