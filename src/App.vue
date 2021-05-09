<template>
  <div id="app">
    <div class=combination-area>
      <ingredient :ingredient="firstPick" :pickable="false"/>
      <ingredient :ingredient="secondPick" :pickable="false"/>
      <div>=</div>
      <ingredient 
        :ingredient="currentResult"
        @pickFirst="firstPick=currentResult"
        @pickSecond="secondPick=currentResult"
        />
    </div>
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <ingredient 
      v-for="ingredient in ingredients" 
      :ingredient="ingredient" 
      :key="ingredient.name"
      @pickFirst="firstPick=ingredient"
      @pickSecond="secondPick=ingredient"
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
  blankIngredient: createIngredient('?'),
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
      let [first, second] = this.firstPick.name.localeCompare(this.secondPick.name) < 0
      ? [this.firstPick.name, this.secondPick.name] 
      : [this.secondPick.name, this.firstPick.name];

      return this.findIngredientByName(this.staticValues.combinations?.[first]?.[second]) ?? this.staticValues.blankIngredient;
    }
  },
  methods:{
    /**
     * Finds an ingredient given it's name. Returns null if no ingredient is found with that name
     * @param {string} name The name of the ingredient to find
     */
    findIngredientByName(name){
      for(let ingredient of this.ingredients){
        if(ingredient.name === name){
          return ingredient
        }
      }
      return null;
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

<style lang="scss">
// Reset css
button{
  font-size: 1rem;
  border-radius: 0;
  border: 1px solid black;
  background: none;
  margin: 0;
}
</style>