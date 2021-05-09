<template>
  <div :class="$options.name">
      <p>{{ingredient.name}}</p>
      <div class="buttons" v-if="pickable">
        <button class="pickFirst" type='button' @click="$emit('pickFirst')">L</button>
        <button class="pickSecond" type='button' @click="$emit('pickSecond')">R</button>
      </div>
  </div>
</template>

<script>
//import { ref } from '@vue/composition-api';

export default {
  name: 'ingredient',
  props:{
    ingredient: Object,
    // If this ingredient should show pick buttons
    pickable: {
      type: Boolean,
      default: true
    }
  },
  setup(){
  }
}
</script>

<style lang="scss">
.ingredient{
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  align-content: space-between;

  box-sizing: border-box;
  border: 1px solid black;
  border-radius: 4px;
  width: 5em;
  height: 5em;

  overflow: hidden;

  .buttons{
    display: flex;
    flex-basis: 100%;
    >button{
      flex: 1;
      border-bottom: none;
      border-left: none;
      background: rgb(126, 61, 0);
      color: rgb(255, 219, 121);
    }
    >.pickSecond{
      border-right: none;
    }
  }
}
</style>
