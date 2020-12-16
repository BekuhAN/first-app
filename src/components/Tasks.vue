<template>
  <div class="tasks-list">
    <div class="task" v-for="list of tasks" :key="list.id">
      <h2 class="task__title" :style="{ color: list.color.hex }">
        {{ list.name }}
        <EditListName :list="list" />
      </h2>
      <ul class="task__items">
        <li class="task__item" v-for="task of list.tasks" :key="task.id">
          <input :id="task.id" :checked="task.completed" />
          <label
            :for="task.id"
            class="task__item__icon"
            :class="{ completed: task.completed }"
            @click="isCompleted(task), (task.completed = !task.completed)"
            >&#10004;</label
          >
          <span class="task__item__text">{{ task.text }}</span>
          <span class="task__item__remove" @click="taskRemove(task)"
            ><img :src="removeIcon"
          /></span>
        </li>
      </ul>
      <NewTask :list="list" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import removeIcon from "../assets/Remove.png";
import NewTask from "./NewTask";
import EditListName from "./EditListName";

export default {
  props: ["tasks"],
  data() {
    return {
      removeIcon,
    };
  },
  methods: {
    isCompleted: function(task) {
      axios.patch(`http://localhost:3000/tasks/${task.id}`, {
        completed: !task.completed,
      });
    },
    taskRemove: function(task) {
      axios
        .delete(`http://localhost:3000/tasks/${task.id}`)
        .catch(() => alert("Error"));
      this.$emit("task-delete", task);
    },
  },
  components: {
    NewTask,
    EditListName,
  },
};
</script>

<style lang="scss" scoped>
.tasks-list {
  width: 100%;
  margin: 50px;
  overflow-y: hidden;
  transition: 0.2s;
  &::-webkit-scrollbar {
    -webkit-appearance: none;
    width: 5px;
  }
  &::-webkit-scrollbar-thumb {
    border-radius: 2px;
    background-color: rgb(214, 214, 214);
  }
  &:hover {
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    -moz-overflow-scrolling: touch;
    -ms-overflow-scrolling: touch;
    -o-overflow-scrolling: touch;
  }
  .task {
    margin-bottom: 50px;
    padding-right: 15px;
    &__title {
      font-size: 36px;
      margin: 0;
      display: flex;
      position: relative;
    }
    &__items {
      list-style: none;
      margin: 50px 0 0;
      padding: 0;
      .task__item {
        display: flex;
        align-items: center;
        position: relative;
        margin: 10px 0;
        transition: 0.2s;
        cursor: pointer;
        input {
          display: none;
        }
        &__icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: 20px;
          height: 20px;
          border-radius: 50%;
          border: 2px solid #e8e8e8;
          min-width: 20px;
          margin-right: 15px;
          font-size: 12px;
          color: #ffffff;
          &.completed {
            background: #4dd599;
            border-color: #4dd599;
          }
        }
        &__remove {
          position: absolute;
          right: 0;
          display: none;
          transition: 0.2s;
        }
        &:hover {
          .task__item {
            &__icon {
              color: #b2b2b2;
              background: #e8e8e8;
              border-color: #e8e8e8;
            }
            &__remove {
              display: block;
            }
          }
        }
      }
    }
  }
}
</style>
