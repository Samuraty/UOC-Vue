<template>
  <div id="recipe-list" class="recipe-list">
    <div v-for="recipe in recipeList" :key="recipe.id">
      <recipe :recipe="recipe" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import Recipe from "./Recipe.vue";

export default defineComponent({
  name: "RecipeList",
  props: {
    recipeList: {
      type: Array,
      required: true,
    },
  },
  components: { Recipe },
});
</script>

<style scoped>
.recipe-list {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr;
}
</style>
