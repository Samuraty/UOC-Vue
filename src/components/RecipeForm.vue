<template>
  <div class="modal-container">
    <div class="recipe-form">
      <div class="recipe-form-header">
        <h2>Add a new recipe</h2>
        <button @click="closeForm">
          <img src="../assets/close-button.svg" alt="Close modal" />
        </button>
      </div>
      <div class="error" v-if="error">
        The fields name, ingredients and directions are required.
      </div>
      <form @submit.prevent="addRecipe">
        <div class="recipe-form-item">
          <label for="title">Title</label>
          <input type="text" id="title" v-model="recipe.title" />
        </div>
        <div class="recipe-form-item">
          <label for="imageUrl">Image URL</label>
          <input type="text" id="imageUrl" v-model="recipe.imageUrl" />
        </div>
        <div class="recipe-form-item">
          <label for="servings">Servings</label>
          <input type="number" id="servings" v-model="recipe.servings" />
        </div>
        <div class="recipe-form-item">
          <label for="time">Time</label>
          <input type="text" id="time" v-model="recipe.time" />
        </div>
        <div class="recipe-form-item">
          <label for="difficulty">Difficulty</label>
          <select id="difficulty" v-model="recipe.difficulty">
            <option value="easy">Easy</option>
            <option value="medium">Medium</option>
            <option value="hard">Hard</option>
          </select>
        </div>
        <div class="recipe-form-item">
          <label for="ingredients">Ingredients</label>
          <textarea type="text" id="ingredients" v-model="recipe.ingredients" />
        </div>
        <div class="recipe-form-item">
          <label for="directions">Directions</label>
          <textarea type="text" id="directions" v-model="recipe.directions" />
        </div>
        <div class="recipe-form-item">
          <label for="featured">Featured Recipe</label>
          <input type="checkbox" id="featured" v-model="recipe.featured" />
        </div>
        <div class="recipe-form-item">
          <button type="submit">Add Recipe</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { defineEmits, ref } from "vue";
import { uuid } from "vue-uuid";

const recipe = ref({
  id: "",
  title: "",
  imageUrl: "",
  servings: "",
  time: "",
  difficulty: "",
  ingredients: [],
  directions: [],
});
const error = ref(false);

const emit = defineEmits(["add-recipe", "close-form"]);

const addRecipe = () => {
  if (
    recipe.value.title.length === 0 ||
    recipe.value.ingredients.length === 0 ||
    recipe.value.directions.length === 0
  ) {
    error.value = true;
    return;
  }
  recipe.value.id = uuid.v4();
  recipe.value.ingredients = recipe.value.ingredients.split(",");
  recipe.value.directions = recipe.value.directions.split(",");
  emit("add-recipe", recipe.value);
  resetRecipe();
};

const resetRecipe = () => {
  recipe.value = {
    id: "",
    title: "",
    imageUrl: "",
    servings: "",
    time: "",
    difficulty: "",
    ingredients: [],
    directions: [],
  };
};

const closeForm = () => {
  emit("close-modal");
};
</script>

<style scoped>
.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 10;
}
.recipe-form {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  max-width: 500px;
  background: #fff;
  padding: 20px;
  border-radius: 5px;
}
.recipe-form-header button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: 0;
  padding: 0;
  cursor: pointer;
}
.recipe-form-header button img {
  width: 20px;
}
.recipe-form-item {
  margin-bottom: 20px;
}
.recipe-form-item label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}
.recipe-form-item input,
.recipe-form-item select,
.recipe-form-item textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  max-width: 300px;
}
.recipe-form-item button {
  background: #4caf50;
  color: #fff;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.error {
  color: red;
  margin-bottom: 20px;
}
</style>
