<template>
  <div id="app">
    <div class=combination-area>
      <ingredient :ingredient="firstPick"/>
      <ingredient :ingredient="secondPick"/>
      <div>=</div>
      <ingredient :ingredient="staticValues.blankIngredient"/>
    </div>
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <ingredient 
      v-for="ingredient in ingredients" 
      :ingredient="ingredient" 
      :key="ingredient.name"
      @click.native.left="firstPick=ingredient"
      @click.native.right.prevent="secondPick=ingredient"
      />
  </div>
</template>

<script>
import Ingredient from './components/Ingredient.vue';

/**
 * Creates an ingredient object with appropriate structure.
 * @param {string} name the name of the ingredient
 */
function createIngredient(name){
  return {
    name: name
  }
}
const staticValues = {
  /**
   * The blank ingredient, used for an unknown or empty ingredient space
   */
  blankIngredient: {
    name: "?"
  },
  /**
   * Defines which combinations have what outputs, by ingredient id.
   * Combinations are commutative, so they are stored by key of first element by alphabetical sorting, and then by the second element
   */
  combinations:{
    'Air':{
      'Air': 'Air',
      'Earth': 'Air',
      'Fire': 'Air',
      'Water': 'Air'
    },
    'Earth':{
      'Earth': 'Earth',
      'Fire': 'Earth',
      'Water': 'Earth'
    },
    'Fire':{
      'Fire': 'Fire',
      'Water': 'Fire'
    },
    'Water':{
      'Water': 'Water'
    }
  }
}

export default {
  name: 'App',
  components: {
    Ingredient
  },
  data(){
    return {
      staticValues,
      // A list of the known ingredients. Must be ingredients objects
      ingredients: [
        createIngredient("Air"),
        createIngredient("Earth"),
        createIngredient("Fire"),
        createIngredient("Water"),
      ],
      firstPick: staticValues.blankIngredient,
      secondPick: staticValues.blankIngredient,
    }
  },
  computed:{
    currentResult(){
      let [first, second] = this.firstPick.name.localeCompare(this.secondPick.name) 
      ? [this.firstPick.name, this.secondPick.name] 
      : [this.secondPick.name, this.firstPick.name];
      return this.ingredients[this.combinations[first][second]];
    }
  }
}
</script>

<style scoped lang="scss">
.combination-area{
  display: flex;
  align-items: center;
  gap: 1em;
}
</style>