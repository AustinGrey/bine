<template>
  <div :class="$options.name">
      <p>{{name}}</p>
  </div>
</template>

<script>
import { ref } from '@vue/composition-api';

export default {
  name: 'ingredient',
  setup(){
      const name = ref("My Element");

      return { name: name }
  }
}
</script>

<style>
</style>
