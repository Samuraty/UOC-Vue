<template>
  <div id="app">
    <div class="header">
      <img class="logo" alt="UOC logo" src="./assets/uoc-logo.png" />
      <div class="app-name">Recipe book</div>
    </div>
    <SearchBar v-on:showForm="toggleForm" v-on:search="setSearchTerm" />
    <RecipeList
      :recipeList="recipeListFiltered"
      v-on:deleteRecipe="deleteRecipe"
    />
    <RecipeForm
      v-on:addRecipe="addRecipe"
      v-on:closeModal="toggleForm"
      v-if="showModal"
    />
  </div>
</template>

<script>
import axios from "axios";
import { defineComponent } from "vue";
import RecipeForm from "./components/RecipeForm.vue";
import RecipeList from "./components/RecipeList.vue";
import SearchBar from "./components/SearchBar.vue";

export default defineComponent({
  name: "App",
  components: {
    RecipeList: RecipeList,
    RecipeForm,
    SearchBar,
  },
  data: () => ({
    recipeList: [],
    showModal: false,
    searchTerm: "",
  }),
  mounted() {
    this.fetchRecipes();
  },
  methods: {
    async addRecipe(recipe) {
      this.recipeList = [...this.recipeList, recipe];
      const req = await axios.post("http://localhost:3000/recipe", recipe);
      if (req.status === 201) {
        this.fetchRecipes();
        this.toggleForm();
      }
    },
    async deleteRecipe(recipeId) {
      this.recipeList = this.recipeList.filter(
        (recipe) => recipe.id !== recipeId
      );
      const req = await axios({
        method: "DELETE",
        url: `http://localhost:3000/recipe/`,
        data: {
          id: recipeId,
        },
      });
      if (req.status === 200) {
        this.fetchRecipes();
      }
    },
    toggleForm() {
      this.showModal = !this.showModal;
    },
    setSearchTerm(searchTerm) {
      this.searchTerm = searchTerm;
    },
    async fetchRecipes() {
      const req = await axios.get("http://localhost:3000/recipes");
      this.recipeList = req.data.recipes;
    },
  },
  computed: {
    recipeListFiltered() {
      if (!this.searchTerm) {
        return this.recipeList;
      }
      return this.recipeList.filter((recipe) => {
        return (
          recipe.title.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
          recipe.ingredients
            .map((ingredient) => ingredient.toLowerCase())
            .includes(this.searchTerm.toLowerCase())
        );
      });
    },
  },
});
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-wrap: wrap;
  height: 100%;
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
}
.header {
  width: 100%;
  padding: 15px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ccc;
}
.header .logo {
  max-height: 50px;
}
.header .app-name {
  margin-left: 25px;
  font-weight: bold;
  font-size: 20px;
}
</style>
