<template>
  <div id="app">
    <div class="header">
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
      <p>Discovered {{numDiscovered}} / {{ingredients.length}}</p>
    </div>
    <div class="ingredients-list">
      <ingredient 
        v-for="ingredient in ingredients" 
        :ingredient="ingredient" 
        :key="ingredient.name"
        @pickFirst="firstPick=ingredient"
        @pickSecond="secondPick=ingredient"
        />
    </div>
  </div>
</template>

<script>
import Ingredient from './components/Ingredient.vue';

/**
 * @typedef {object} IngredientOptions
 * @property {boolean} discovered If this ingredient is discovered
 */

/**
 * Creates an ingredient object with appropriate structure.
 * @param {string} name the name of the ingredient
 * @param {IngredientOptions} options the optional features of this ingredient
 */
function createIngredient(name, options){
  return {
    ...{ // Defaults
      discovered: false
    },
    ...options,
    name
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
      'Earth': 'Dust',
      'Fire': 'Fire',
      'Water': 'Cloud'
    },
    'Earth':{
      'Fire': 'Earth',
      'Water': 'Mud'
    },
    'Fire':{
      'Water': 'Steam'
    },
    'Water':{
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
        // The basic four elements
        createIngredient("Air", {discovered: true}),
        createIngredient("Earth", {discovered: true}),
        createIngredient("Fire", {discovered: true}),
        createIngredient("Water", {discovered: true}),

        // The combinations of the basic elements
        createIngredient("Dust"),
        createIngredient("Cloud"),
        createIngredient("Mud"),
        createIngredient("Steam"),
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

      // Find a specific result's name if there is one
      let resultName = this.staticValues.combinations?.[first]?.[second];

        // If no specifically named result is provided, perform some basic heuristics
      if(resultName === undefined){
        // A + _ == _ + A == _
        if(first === this.staticValues.blankIngredient.name || second === this.staticValues.blankIngredient.name) return this.staticValues.blankIngredient;

        // A + A = A
        if(first === second) resultName = first;
      }

      // If there still is no result name, just return the blank ingredient
      if(resultName === undefined) return this.staticValues.blankIngredient;

      let ingredient = this.findIngredientByName(resultName);

      // If this ingredient cannot be found by name, warn the developer and just return the blank ingredient
      if(ingredient === null){
        console.error(`Ingredient ${resultName} was not found in the list of ingredients`);
        return this.staticValues.blankIngredient
      }

      // Ensure this ingredient is marked as discovered
      ingredient.discovered = true;


      return  ingredient;
    },
    /**
     * The number of ingredients that have been discovered
     */
    numDiscovered(){
      return this.ingredients.reduce((accumulator, ingredient)=>accumulator + (ingredient.discovered ? 1 : 0), 0)
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
.header{
  position: sticky;
  top: 0;
  background: white;
  border-bottom: 1px solid black;
  margin-bottom: 0.5em;
}

.ingredients-list{
  display: flex;
  flex-wrap: wrap;
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