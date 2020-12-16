<template>
  <div class="task__add">
    <div class="task__new" @click="showForm()">
      <span class="task__new__icon"><img :src="iconAddList" alt=""/></span>
      Новая задача
    </div>
    <form class="task__form" v-if="showAddTask" @submit.prevent="addTask">
      <input
        class="task__form__text"
        type="text"
        placeholder="Введите текст"
        v-model="taskText"
        required
      />
      <input class="task__form__add btn" type="submit" value="Добавить" />
      <input
        class="task__form__cancel btn"
        type="button"
        value="Отмена"
        @click="showForm()"
      />
    </form>
  </div>
</template>

<script>
import axios from "axios";
import iconAddList from "../assets/Group.png";

export default {
  props: ["list"],
  data() {
    return {
      iconAddList,
      showAddTask: false,
      taskText: "",
    };
  },
  methods: {
    showForm: function() {
      this.showAddTask = !this.showAddTask;
      this.taskText = "";
    },
    addTask: function() {
      const newTask = {
        listId: this.list.id,
        text: this.taskText,
        completed: false,
      };
      axios
        .post("http://localhost:3000/tasks/", newTask)
        .then(({ data }) => {
          this.$root.$children[0].addNewTask(data);
          this.showForm();
        })
        .catch(() => alert("error"));
    },
  },
};
</script>

<style lang="scss" scoped>
.task__add {
  position: relative;
}
.task__new {
  display: flex;
  align-items: center;
  color: #b4b4b4;
  &__icon {
    width: 15px;
    height: 15px;
    margin-right: 17px;
    margin-left: 3px;
    img {
      width: 100%;
      height: 100%;
    }
  }
}
.task__form {
  position: absolute;
  top: 0;
  left: 0;
  background: #ffffff;
  &__text {
    padding: 8px 10px;
    border: 1px solid #eeeeee;
    border-radius: 5px;
    width: 100%;
    margin: 10px 0 20px;
  }
  .btn {
    border: none;
    padding: 10px 15px;
    border-radius: 5px;
    margin-right: 10px;
    cursor: pointer;
    transition: 0.2s;
  }
  &__add {
    background: #4dd599;
    color: #ffffff;
    &:hover {
      background: lighten(#4dd599, 10%);
    }
  }
  &__cancel {
    background: #f4f6f8;
    color: #9c9c9c;
    &:hover {
      background: darken(#f4f6f8, 10%);
    }
  }
}
</style>
