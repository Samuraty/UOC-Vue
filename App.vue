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
    recipeList: [
      {
        id: 1,
        servings: 4,
        time: "30m",
        difficulty: "Easy",
        title: "Spaghetti",
        ingredients: ["noodles", "tomato sauce", "cheese"],
        directions: ["boil noodles", "cook noodles", "eat noodles"],
        imageUrl:
          "https://imagesvc.meredithcorp.io/v3/mm/image?q=60&c=sc&poi=face&w=2000&h=1000&url=https%3A%2F%2Fstatic.onecms.io%2Fwp-content%2Fuploads%2Fsites%2F21%2F2018%2F02%2F14%2Frecetas-4115-spaghetti-boloesa-facil-2000.jpg",
      },
      {
        id: 2,
        servings: 2,
        time: "45m",
        difficulty: "Medium",
        title: "Pizza",
        ingredients: ["dough", "tomato sauce", "cheese"],
        directions: ["boil dough", "cook dough", "eat pizza"],
        imageUrl:
          "https://imagesvc.meredithcorp.io/v3/mm/image?q=60&c=sc&poi=face&w=2000&h=1000&url=https%3A%2F%2Fstatic.onecms.io%2Fwp-content%2Fuploads%2Fsites%2F21%2F2018%2F02%2F14%2Frecetas-4115-spaghetti-boloesa-facil-2000.jpg",
      },
      {
        id: 3,
        servings: 6,
        time: "1h",
        difficulty: "Hard",
        title: "Lasagna",
        ingredients: ["noodles", "tomato sauce", "cheese"],
        directions: ["boil noodles", "cook noodles", "eat noodles"],
        imageUrl:
          "https://imagesvc.meredithcorp.io/v3/mm/image?q=60&c=sc&poi=face&w=2000&h=1000&url=https%3A%2F%2Fstatic.onecms.io%2Fwp-content%2Fuploads%2Fsites%2F21%2F2018%2F02%2F14%2Frecetas-4115-spaghetti-boloesa-facil-2000.jpg",
      },
    ],
    showModal: false,
    searchTerm: "",
  }),
  methods: {
    addRecipe(recipe) {
      this.recipeList = [...this.recipeList, recipe];
      this.toggleForm();
    },
    deleteRecipe(recipeId) {
      this.recipeList = this.recipeList.filter(
        (recipe) => recipe.id !== recipeId
      );
    },
    toggleForm() {
      this.showModal = !this.showModal;
    },
    setSearchTerm(searchTerm) {
      this.searchTerm = searchTerm;
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
