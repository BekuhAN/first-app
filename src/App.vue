<template>
  <div id="app">
    <div class="sidebar">
      <List
        v-if="db.length"
        :lists="[...allList, ...db]"
        @on-delete="listDelete"
        @selected-tasks="selectedTasks"
      />
      <NewList :colors="colors" @add-new-list="addNewList" />
    </div>
    <Tasks v-if="tasks.length" :tasks="tasks" @task-delete="taskDelete" />
    <div v-else class="task__undefiend">Задачи отсутствуют</div>
  </div>
</template>

<script>
import axios from "axios";
import List from "./components/List";
import NewList from "./components/NewList";
import Tasks from "./components/Tasks";
import iconAllList from "./assets/Vector.png";

export default {
  name: "App",
  data() {
    return {
      db: [],
      colors: [],
      tasks: [],
      allList: [
        {
          name: "Все задачи",
          icon: iconAllList,
          notRemoveble: true,
        },
      ],
    };
  },
  mounted() {
    axios
      .get("http://localhost:3000/lists?_embed=tasks&_expand=color")
      .then((resp) => {
        this.db = resp.data;
        this.tasks = this.db;
      });
    axios
      .get("http://localhost:3000/colors")
      .then((resp) => (this.colors = resp.data));
  },
  methods: {
    addNewList: function(newList) {
      this.db.push(newList);
    },
    listDelete: function(list) {
      this.db = this.db.filter((item) => item.id !== list.id);
      this.tasks = this.db;
    },
    selectedTasks: function(list) {
      list.notRemoveble ? (this.tasks = this.db) : (this.tasks = [list]);
    },
    addNewTask: function(task) {
      this.db.find((i) => i.id === task.listId).tasks.push(task);
    },
    taskDelete: function(task) {
      this.db.map((list) => {
        if (list.id === task.listId) {
          list.tasks = list.tasks.filter((item) => item.id !== task.id);
        }
        return list;
      });
    },
    editListName: function(newList) {
      this.db.map((list) => {
        if (list.id === newList.id) {
          list.name = newList.name;
        }
        return list;
      });
    },
  },
  components: {
    List,
    NewList,
    Tasks,
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}

#app {
  display: flex;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 746px;
  height: 529px;
  box-shadow: 0 0 5px 0px #eee;
  .sidebar {
    width: 200px;
    flex: 0 0 200px;
    background: #f4f6f8;
    height: 100%;
    padding: 50px 20px;
  }
  .task__undefiend {
    color: #c9d1d3;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 32px;
  }
}
</style>
