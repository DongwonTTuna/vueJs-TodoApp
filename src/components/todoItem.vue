<template>
    <table class="todoItem" >
      <tr>
        <td class="todoItem__title">
          {{ todoItem.title }}
        </td>
        <td class="todoItem__description">
          {{ todoItem.description }}
        </td>
        <td>
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

<style></style>

<script lang="ts">
import { PropType } from "vue";

interface todoItem {
  title: string;
  description: string;
  expireDate: Date;
}
export default {
  name: "TodoItem",
  data() {
    return {
      daysOfWeek: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
      // Other data properties
    };
  },
  props: {
    todoItem: {
      type: Object as PropType<todoItem>,
      required: true,
    },
  },
};
</script>
