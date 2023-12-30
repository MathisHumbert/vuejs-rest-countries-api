<script setup>
import { defineProps } from 'vue';
import { RouterLink } from 'vue-router';

const { id, image, alt, name, population, region, capital } = defineProps([
  'id',
  'image',
  'alt',
  'name',
  'population',
  'region',
  'capital',
]);
</script>

<template>
  <RouterLink :to="`/detail/${id}`">
    <article class="country">
      <img :src="image" :alt="alt" class="country__img" />
      <div class="country__container">
        <h2 class="country__title">{{ name }}</h2>
        <div class="country__infos">
          <div class="country__infos__container">
            <p class="country__infos__title">Population:</p>
            &nbsp;
            <p class="country__infos__text">{{ population }}</p>
          </div>
          <div class="country__infos__container">
            <p class="country__infos__title">Region:</p>
            &nbsp;
            <p class="country__infos__text">{{ region }}</p>
          </div>
          <div class="country__infos__container">
            <p class="country__infos__title">Capital:</p>
            &nbsp;
            <p class="country__infos__text">{{ capital ? capital[0] : '-' }}</p>
          </div>
        </div>
      </div>
    </article>
  </RouterLink>
</template>

<style scoped>
a {
  display: block;
  width: 100%;
  height: 100%;
  color: var(--secondary-color);
  text-decoration: none;
}

.country {
  height: 100%;
  background: var(--primary-color);
  border-radius: 5px;
  overflow: hidden;
}

.country__img {
  width: 100%;
  height: 160px;
  object-fit: cover;
}

.country__title {
  font-size: 18px;
  font-weight: 800;
  margin-bottom: 16px;
}

.country__container {
  padding: 24px;
}

.country__infos {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.country__infos__container {
  display: flex;
}

.country__infos__title {
  font-size: 14px;
  font-weight: 600;
}

.country__infos__text {
  font-size: 14px;
  font-weight: 300;
}
</style>
