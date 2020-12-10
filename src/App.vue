<template>
  <div id="app">
    <div class="sidebar">
      <List :lists="[...allList, ...db]" />
      <NewList :colors="colors" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import List from "./components/List";
import NewList from "./components/NewList";
import iconAllList from "./assets/Vector.png";

export default {
  name: "App",
  data() {
    return {
      db: [],
      colors: [],
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
      .get("http://localhost:3000/lists?_expand=color&_embed=tasks")
      .then((resp) => (this.db = resp.data));
      
    axios
      .get("http://localhost:3000/colors")
      .then((resp) => (this.colors = resp.data));
  },
  methods: {
    handleActive: function(id) {
      this.isActive = id;
    },
  },
  components: {
    List,
    NewList
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}

#app {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 746px;
  height: 529px;
  box-shadow: 0 0 5px 0px #eee;
  .sidebar {
    width: 200px;
    background: #f4f6f8;
    height: 100%;
    padding: 50px 20px;
  }
}
</style>
