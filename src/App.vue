<template>
  <div class="container animated fadeIn">
    <div class="row">
      <h1 class="title">Random Meal Generator</h1>
    </div>
    <hr />
    <div class="row">
      <button class="btn btn-primary" @click="getMeal">
        Get Meal 🍔
      </button>
    </div>
    <br />
    <transition
      enter-active-class="animated fadeIn"
      leave-active-class="animated fadeOut"
    >
      <meal-display v-if="mealActive" :meal="meal"></meal-display>
    </transition>
  </div>
</template>

<script>
import MealDisplay from "./components/MealDisplay.vue";

export default {
  components: {
    MealDisplay
  },
  data() {
    return {
      mealActive: false,
      meal: {
        name: "",
        instructions: "",
        youtubeLink: "",
        ingredients: [],
        measurements: [],
        ingredientMeasure: [],
        mealImg: ""
      }
    };
  },
  methods: {
    clearData() {
      this.meal = {
        name: "",
        instructions: "",
        youtubeLink: "",
        ingredients: [],
        measurements: [],
        ingredientMeasure: [],
        mealImg: ""
      };
    },
    getMeal() {
      if (this.mealActive) {
        this.mealActive = false;
        this.clearData();
        return;
      }
      this.mealActive = true;
      this.$http
        .get("https://www.themealdb.com/api/json/v1/1/random.php")
        .then(response => {
          console.log(response);
          this.meal.name = response.data.meals[0].strMeal;
          this.meal.instructions = response.data.meals[0].strInstructions;
          this.meal.youtubeLink = response.data.meals[0].strYoutube;
          this.meal.mealImg = response.data.meals[0].strMealThumb;
          //get ingredients and measurements
          const keys = Object.entries(response.data.meals[0]);
          for (let i = 0; i < keys.length; i++) {
            if (keys[i][0].includes("strIngredient") && keys[i][1].length > 0) {
              this.meal.ingredients.push(keys[i][1]);
            } else if (
              keys[i][0].includes("strMeasure") &&
              keys[i][1].length > 0
            ) {
              this.meal.measurements.push(keys[i][1]);
            }
          }
          //neatly put name of ingredient and measurement into array
          for (let i = 0; i < this.meal.ingredients.length; i++) {
            this.meal.ingredientMeasure.push([
              this.meal.ingredients[i],
              this.meal.measurements[i]
            ]);
          }
        });
    }
  }
};
</script>

<style>
.title,
.btn,
.card {
  text-align: center;
  margin: 0 auto;
}
</style>
