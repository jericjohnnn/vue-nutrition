<template>
  <div>
    <select v-model="selectedRecipeId" @change="loadIngredients">
      <option v-for="recipe in recipes" :key="recipe.id" :value="recipe.id">
        {{ recipe.name }}
      </option>
    </select>

    <ul v-if="ingredients.length">
      <li v-for="ingredient in ingredients" :key="ingredient.id">
        {{ ingredient.name }}
      </li>
    </ul>

    <input v-model.number="numServings" type="number" placeholder="Number of servings">
    <input v-model.number="servingSize" type="number" placeholder="Serving size">

    <button @click="calculateNutrition">Calculate</button>

    <div v-if="nutritionValues">
      <h3>Nutrition Values:</h3>
      <p>Calories: {{ nutritionValues.calories }}</p>
      <p>Fat: {{ nutritionValues.fat }}g</p>
      <p>Carbs: {{ nutritionValues.carbs }}g</p>
      <p>Protein: {{ nutritionValues.protein }}g</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      recipes: [],
      selectedRecipeId: null,
      ingredients: [],
      numServings: 1,
      servingSize: 100,
      nutritionValues: null
    }
  },
  mounted() {
    this.loadRecipes();
  },
  methods: {
    async loadRecipes() {
      const response = await axios.get('/api/recipes');
      this.recipes = response.data;
    },
    async loadIngredients() {
      if (this.selectedRecipeId) {
        const response = await axios.get(`/api/recipes/${this.selectedRecipeId}/ingredients`);
        this.ingredients = response.data;
      }
    },
    async calculateNutrition() {
      const response = await axios.post('/api/calculate-nutrition', {
        recipe_id: this.selectedRecipeId,
        num_servings: this.numServings,
        serving_size: this.servingSize
      });
      this.nutritionValues = response.data;
    }
  }
}
</script>