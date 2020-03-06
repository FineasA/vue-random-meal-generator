<template>
  <div class="card" style="width: 50%; text-align: justify">
    <img :src="meal.mealImg" class="card-img-top" :alt="meal.name" />

    <div class="card-body">
      <h4 class="card-text" style="text-align: center">
        {{ meal.name }}
      </h4>
      <a
        style="display: block; text-align:center; margin: 0 auto;"
        class="card-link"
        :href="meal.youtubeLink"
        >Watch Video!</a
      >
      <hr />
      <h3 style="text-align:center" class="card-text">Instructions</h3>
      <p class="card-text">{{ meal.instructions }}</p>
      <hr />
      <h3 style="text-align: center" class="card-text">Ingredients</h3>
      <ul class="list-group list-group-flush">
        <li
          v-for="(ingredient, index) in meal.ingredientMeasure"
          :key="index"
          class="list-group-item"
        >
          {{ ingredient[1] }} {{ ingredient[0] }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    meal: Object
  }
};
</script>

<style></style>
