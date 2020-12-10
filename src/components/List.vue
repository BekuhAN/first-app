<template>
  <ul class="lists">
    <li class="lists__item" @click="handleActive(i)" 
        :class="{active: i === isActive}" 
        v-for="(list, i) of lists" 
        :key="i">
      <span v-if="list.icon" 
            :style="{ background: `url(${list.icon})` }"></span>
      <span v-else 
            :style="{ background: list.color.hex }" class="list__item"></span>
      {{ list.name }}
      <span v-if="!list.notRemoveble && i === isActive" 
            :style="{ background: `url(${removeIcon})` }" class="list__remove"></span>
    </li>
  </ul>
</template>

<script>
import removeIcon from '../assets/Remove.png';

export default {
  props: ["lists"],
  data(){ 
    return {
      isActive: 0,
      removeIcon
    }
  },
  methods: {
    handleActive: function(i) {
      this.isActive = i
    }
  }
};
</script>

<style lang="scss" scoped>
.lists {
    margin: 0 0 20px;
    padding: 0;
    list-style: none;
    font-size: 16px;
    &__item {
        display: flex;
        align-items: center;
        padding: 10px 0 10px 10px;
        position: relative;
      &.active{
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
      &.list__item {
          border-radius: 50%;
        }
      &.list__remove {
        position: absolute;
        right: 0;
        
      }
    }
}

</style>

