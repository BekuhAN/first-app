<template>
  <div class="task__title__edit">
    <span class="task__title__edit_icon" @click="showEditForm()">&#9998;</span>
    <form
      class="task__title__edit_form"
      v-if="showEdit"
      @submit.prevent="editList"
    >
      <input
        class="task__title__edit_form__name"
        type="text"
        placeholder="Введите текст"
        v-model="newListName"
        required
      />
      <input
        class="task__title__edit_form__edit btn"
        type="submit"
        value="Изменить"
      />
      <input
        class="task__title__edit_form__cancel btn"
        type="button"
        value="Отмена"
        @click="showEditForm()"
      />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: ["list"],
  data() {
    return {
      showEdit: false,
      newListName: this.list.name,
    };
  },
  methods: {
    showEditForm: function() {
      this.showEdit = !this.showEdit;
    },
    editList: function() {
      axios
        .patch(`http://localhost:3000/lists/${this.list.id}`, {
          name: this.newListName,
        })
        .then(({ data }) => {
          this.$root.$children[0].editListName(data);
          this.showEditForm();
        })
        .catch(() => alert("errror"));
    },
  },
};
</script>

<style lang="scss" scoped>
.task__title {
  &__edit {
    &_icon {
      color: #e8e8e8;
      display: none;
      transform: scaleX(-1);
      font-size: 28px;
      cursor: pointer;
      transition: .2s;
      margin-left: 15px;
    }
    &_form {
      position: absolute;
      width: 100%;
      left: 0;
      background: #ffffff;
      z-index: 4;
      &__name {
        padding: 8px 10px;
        border: 1px solid #eeeeee;
        border-radius: 5px;
        width: 100%;
        margin: 5px 0;
      }
      .btn {
        border: none;
        padding: 10px 15px;
        border-radius: 5px;
        margin-right: 10px;
        cursor: pointer;
        transition: 0.2s;
      }
      &__edit {
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
  }
  &:hover {
    .task__title__edit_icon {
      display: inline-block;
    }
  }
}
</style>
