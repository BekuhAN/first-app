<template>
  <div class="new_list">
    <span class="list__add" @click="showForm()">
      <i
        class="list__add__icon"
        :style="{ background: `url(${iconAddList})` }"
      ></i>
      Добавить список
    </span>
    <form class="list__form" v-if="formShow" @submit.prevent="addList">
      <input
        class="list__name"
        type="text"
        placeholder="Название списка"
        v-model="listName"
        required
      />
      <ul class="list__colors">
        <li
          class="colors__item"
          :class="{ colors__item__active: color.id === colorItemActive }"
          v-for="color of colors"
          :key="color.id"
          :style="{ background: color.hex }"
          @click="selectColor(color.id)"
        ></li>
      </ul>
      <input class="btn" type="submit" value="Добавить" />
      <span class="list__form__close" @click="showForm()">&times;</span>
    </form>
  </div>
</template>

<script>
import axios from "axios";
import iconAddList from "../assets/Group.png";

export default {
  props: ["colors"],
  data() {
    return {
      iconAddList,
      formShow: false,
      colorItemActive: this.colors.length ? this.colors[0].id : null,
      listName: "",
    };
  },
  methods: {
    selectColor: function(i) {
      this.colorItemActive = i;
    },
    showForm: function() {
      this.listName = "";
      this.formShow = !this.formShow;
      this.colorItemActive = this.colors.length ? this.colors[0].id : null;
    },
    addList: function() {
      const newList = {
        name: this.listName,
        colorId: this.colorItemActive,
      };
      axios
        .post("http://localhost:3000/lists", newList)
        .then(({ data }) => {
          const color = this.colors.find(
            (color) => color.id == this.colorItemActive
          );
          this.$emit("add-new-list", { ...data, color, tasks: [] });
          this.showForm();
        })
        .catch(() => alert("error"));
    },
  },
};
</script>

<style lang="scss" scoped>
.new_list {
  position: relative;
  font-size: 14px;
}

.list__add {
  display: flex;
  align-items: center;
  padding: 10px 0 10px 10px;
  position: relative;
  cursor: pointer;
  &__icon {
    display: block;
    height: 10px;
    width: 10px;
    margin-right: 10px;
    background-size: cover !important;
  }
}
.list__form {
  position: absolute;
  top: 50px;
  background: #ffffff;
  padding: 20px;
  box-sizing: content-box;
  border-radius: 5px;
  box-shadow: 0 0 5px 0px #eee;
  z-index: 4;
  .list__name {
    padding: 8px 10px;
    border: 1px solid #eeeeee;
    border-radius: 5px;
  }
  .list__colors {
    display: flex;
    list-style: none;
    padding: 0;
    justify-content: space-between;
    .colors__item {
      display: block;
      height: 16px;
      width: 16px;
      border-radius: 50%;
      &__active {
        border: 2px solid #525252;
      }
    }
  }
  .btn {
    background: #4dd599;
    border: none;
    padding: 10px 0;
    width: 100%;
    border-radius: 5px;
    color: #ffffff;
    cursor: pointer;
    transition: 0.2s;
    &:hover {
      background: lighten(#4dd599, 10%);
    }
  }
  &__close {
    display: flex;
    width: 30px;
    height: 30px;
    background: #5c5c5c;
    border-radius: 50%;
    color: #ffffff;
    align-items: center;
    justify-content: center;
    position: absolute;
    right: -15px;
    top: -15px;
    cursor: pointer;
    font-size: 20px;
    &:hover {
      background: lighten(#5c5c5c, 10%);
    }
  }
}
</style>
