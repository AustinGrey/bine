<template>
  <div :class="$options.name">
      <p>{{ingredient.name}}</p>
  </div>
</template>

<script>
//import { ref } from '@vue/composition-api';

export default {
  name: 'ingredient',
  props:{
    ingredient: Object
  },
  setup(){
  }
}
</script>

<style lang="scss">
.ingredient{
  border: 1px solid black;
  border-radius: 4px;
  width: 5em;
  height: 5em;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
