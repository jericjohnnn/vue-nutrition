<template>
    <div>
      <h1>Nutrition Calculator</h1>
      <div>
        <label for="recipe-id">Recipe ID:</label>
        <input v-model.number="recipeId" type="number" id="recipe-id" min="1" />
        <button @click="fetchNutrition">Fetch Recipe Info</button>
      </div>
      <div v-if="nutrition">
        <p>Serving Size: {{ nutrition.serving_size }} grams</p>
        <p>Calories: {{ nutrition.calories }} kcal</p>
        <p>Fat: {{ nutrition.fat }} g</p>
        <p>Protein: {{ nutrition.protein }} g</p>
        <p>Carbohydrates: {{ nutrition.carbohydrates }} g</p>
      </div>
      <div>
        <label for="serving-size">Serving Size (grams):</label>
        <input v-model.number="servingSize" type="number" id="serving-size" min="1" />
        <label for="number-of-servings">Number of Servings:</label>
        <input v-model.number="numberOfServings" type="number" id="number-of-servings" min="1" />
        <button @click="calculateNutrition">Calculate</button>
      </div>
      <div v-if="totalNutrition">
        <h2>Total Nutritional Value</h2>
        <p>Calories: {{ totalNutrition.calories.toFixed(2) }} kcal</p>
        <p>Fat: {{ totalNutrition.fat.toFixed(2) }} g</p>
        <p>Protein: {{ totalNutrition.protein.toFixed(2) }} g</p>
        <p>Carbohydrates: {{ totalNutrition.carbohydrates.toFixed(2) }} g</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import axios from 'axios';
  
  const recipeId = ref(1);
  const servingSize = ref(100);
  const numberOfServings = ref(1);
  const nutrition = ref(null);
  const totalNutrition = ref(null);
  
  const fetchNutrition = async () => {
    try {
      const response = await axios.get(`/api/recipes/${recipeId.value}`);
      nutrition.value = response.data;
    } catch (error) {
      console.error(error);
    }
  };
  
  const calculateNutrition = async () => {
    try {
      const response = await axios.post('/api/recipes/calculate', {
        recipeId: recipeId.value,
        servingSize: servingSize.value,
        numberOfServings: numberOfServings.value
      });
      totalNutrition.value = response.data;
    } catch (error) {
      console.error(error);
    }
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  