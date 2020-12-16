<template>
  <ul class="lists">
    <li
      class="lists__item"
      @click="collBack(list)"
      :class="{ active: list.id === isActive.id }"
      v-for="list of lists"
      :key="list.id"
    >
      <span
        v-if="list.icon"
        :style="{ background: `url(${list.icon})` }"
      ></span>
      <span
        v-else
        :style="{ background: list.color.hex }"
        class="list__item"
      ></span>
      <p class="list__name">{{ list.name }}</p>
      <span
        v-if="!list.notRemoveble && list.id === isActive.id"
        :style="{ background: `url(${removeIcon})` }"
        class="list__remove"
        @click="delList"
      ></span>
    </li>
  </ul>
</template>

<script>
import axios from "axios";
import removeIcon from "../assets/Remove.png";

export default {
  props: ["lists"],
  data() {
    return {
      isActive: this.lists[0],
      removeIcon,
    };
  },
  methods: {
    handleActive: function(list) {
      this.isActive = list;
    },
    delList: function() {
      axios
        .delete(`http://localhost:3000/lists/${this.isActive.id}`)
        .then(() => {
          this.$emit("on-delete", this.isActive);
          this.isActive = this.lists[0];
        })
        .catch(() => alert("Error"));
    },
    collBack: function(list) {
      this.handleActive(list);
      this.selectTask();
    },
    selectTask: function() {
      this.$emit("selected-tasks", this.isActive);
    },
  },
};
</script>

<style lang="scss" scoped>
.lists {
  margin: 0 0 20px;
  padding: 0;
  list-style: none;
  font-size: 14px;
  &__item {
    display: flex;
    align-items: center;
    padding: 10px 0 10px 10px;
    position: relative;
    &.active {
      background: #fff;
      border-radius: 3px;
    }
    &:hover {
      cursor: pointer;
    }
    &:first-child {
      margin-bottom: 15px;
    }
  }
  span {
    display: block;
    height: 10px;
    width: 10px;
    margin-right: 10px;
    background-size: cover !important;
    min-width: 10px;
    &.list__item {
      border-radius: 50%;
    }
    &.list__remove {
      position: absolute;
      right: 0;
    }
  }
  .list__name {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    padding-right: 26px;
    margin: 0;
  }
}
</style>
