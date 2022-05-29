<template>
  <div id="recipe-list" class="recipe-list">
    <div v-for="recipe in recipeList" :key="recipe.id">
      <RecipeCard :recipe="recipe" v-on:delete-recipe="deleteRecipe" />
    </div>
  </div>
</template>

<script setup>
import { defineEmits, defineProps } from "vue";
import RecipeCard from "./RecipeCard.vue";

defineProps({
  recipeList: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(["delete-recipe"]);

const deleteRecipe = (id) => {
  emit("delete-recipe", id);
};
</script>

<style scoped>
.recipe-list {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr;
}
</style>
