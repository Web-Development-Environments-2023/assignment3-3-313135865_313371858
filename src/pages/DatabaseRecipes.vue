<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.popularity }}</div>
              <div>Servings: {{ recipe.servings }}</div>
              <b-badge v-if="recipe.glutenFree == 1" variant="primary"
                >Gluten Free</b-badge
              >
              &nbsp;
              <b-badge v-if="recipe.vegetarian == 1" variant="success"
                >Vegetarian</b-badge
              >
              &nbsp;
              <b-badge v-if="recipe.vegan == 1" variant="info">Vegan</b-badge>
            </div>
            Ingredients:
            {{ recipe.extendedIngredients }}
            <ul></ul>
          </div>
          <div class="wrapped">
            Instructions:
            {{ recipe.instructions }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null,
    };
  },

  async created() {
    console.log(this.$route.params.recipeType);
    let response;
    if (this.$route.params.recipeType == "Personal") {
      try {
        console.log(this.$root.store.server_domain + "/users/personalRecipe");
        response = await this.axios.get(
          this.$root.store.server_domain + "/users/personalRecipe",
          { withCredentials: true }
        );
        console.log(response);
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
    } else if (this.$route.params.recipeType == "Family") {
      try {
        console.log(this.$root.store.server_domain + "/users/familyRecipes");
        response = await this.axios.get(
          this.$root.store.server_domain + "/users/familyRecipes",
          { withCredentials: true }
        );
        console.log(response);
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
    }

    console.log(response.data);
    console.log(this.$route.params.recipeId);
    var result = response.data.filter((obj) => {
      return obj.id === this.$route.params.recipeId;
    });

    this.recipe = result[0];
    console.log(this.recipe);

    //   this.recipe =  {
    //     instructions,
    //     extendedIngredients,
    //     popularity,
    //     readyInMinutes,
    //     image,
    //     title
    //   } = result;
  },
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
