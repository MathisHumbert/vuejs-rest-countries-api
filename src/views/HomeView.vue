<script setup>
import { ref, watch, onMounted } from 'vue';

import CountryCard from '../components/CountryCard.vue';

const searchInput = ref('');
const filterInput = ref('');
const countries = ref([]);

const selectOpen = ref(false);

onMounted(async () => {
  const response = await fetch('https://restcountries.com/v3.1/all');
  const data = await response.json();

  countries.value = data;
});

const setFilterValue = (value) => {
  if (filterInput.value === value) {
    filterInput.value = '';
  } else {
    filterInput.value = value;
  }
};

const filterOptions = ['Africa', 'America', 'Asia', 'Europe', 'Oceania'];

watch(filterInput, async () => {
  const response = await fetch('https://restcountries.com/v3.1/all');

  const data = await response.json();

  if (filterInput.value.length > 0) {
    const filteredCountries = data.filter((country) =>
      country.region.includes(filterInput.value)
    );

    countries.value = filteredCountries;
  } else {
    countries.value = data;
  }

  selectOpen.value = false;
});

watch(searchInput, async () => {
  if (searchInput.value.length === 0) {
    const response = await fetch('https://restcountries.com/v3.1/all');

    const data = await response.json();

    countries.value = data;
  } else if (filterInput.value.length > 0) {
    const response = await fetch(
      `https://restcountries.com/v3.1/name/${searchInput.value}`
    );

    const data = await response.json();

    const filteredCountries = data.filter((country) =>
      country.region.includes(filterInput.value)
    );

    countries.value = filteredCountries;
  } else {
    const response = await fetch(
      `https://restcountries.com/v3.1/name/${searchInput.value}`
    );

    const data = await response.json();

    countries.value = data;
  }
});
</script>

<template>
  <div class="home">
    <section class="input__container">
      <div class="search__input">
        <svg
          class="search__input__svg"
          xmlns="http://www.w3.org/2000/svg"
          width="100%"
          height="100%"
          viewBox="0 0 18 18"
          fill="none"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M12.5 11H11.7L11.4 10.7C12.4 9.6 13 8.1 13 6.5C13 2.9 10.1 0 6.5 0C2.9 0 0 2.9 0 6.5C0 10.1 2.9 13 6.5 13C8.1 13 9.6 12.4 10.7 11.4L11 11.7V12.5L16 17.5L17.5 16L12.5 11ZM6.5 11C4 11 2 9 2 6.5C2 4 4 2 6.5 2C9 2 11 4 11 6.5C11 9 9 11 6.5 11Z"
            fill="white"
          />
        </svg>
        <input
          v-model="searchInput"
          type="text"
          placeholder="Search for a country…"
          class="search__input__text"
        />
      </div>
      <div class="filter">
        <div class="filter__input">
          <div @click="selectOpen = !selectOpen" class="filter__input__select">
            <p class="filter__input__select__text">
              {{ filterInput === '' ? 'Filter by Region' : filterInput }}
            </p>
            <svg
              class="filter__input__select__icon"
              :class="{ active: filterInput.length > 0 }"
              xmlns="http://www.w3.org/2000/svg"
              width="100%"
              height="100%"
              viewBox="0 0 12 12"
              fill="none"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M9.45 3.45L6 6.9L2.55 3.45L1.5 4.5L6 9L10.5 4.5L9.45 3.45Z"
                fill="white"
              />
            </svg>
          </div>
          <div class="filter__input__list" :class="{ active: selectOpen }">
            <ul class="filter__input__list__container">
              <li
                v-for="filter in filterOptions"
                @click="setFilterValue(filter)"
                class="filter__input__list__item"
              >
                {{ filter }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>
    <section class="country__container">
      <CountryCard
        v-for="country in countries"
        :key="country.cca2"
        :id="country.cca2"
        :image="country.flags && country.flags.png"
        :alt="country.flags && country.flags.alt"
        :name="country.name && country.name.official"
        :population="country.population"
        :region="country.region"
        :capital="country.capital"
      />
    </section>
  </div>
</template>

<style scoped>
.home {
  padding: 48px 80px;
}
.input__container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.search__input {
  display: flex;
  align-items: center;
  gap: 24px;
  width: 480px;
  height: 56px;
  padding: 20px 32px;
  background: var(--primary-color);
  border: none;
  border-radius: 5px;
}

.search__input__svg {
  width: 18px;
  height: 18px;
}

.search__input__svg path {
  fill: var(--secondary-color);
}

.search__input__text {
  width: 100%;
  height: 20px;
  background: transparent;
  border: none;
}

.search__input__text::placeholder {
  color: var(--secondary-color);
}

.search__input__text:focus {
  outline: none;
}

.country__container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: 75px;
  margin-top: 48px;
}

.filter__input {
  position: relative;
}

.filter__input__select {
  width: 200px;
  height: 56px;
  padding: 0 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--primary-color);
  cursor: pointer;
  border-radius: 5px;
  box-shadow: 0px 2px 9px 0px rgba(0, 0, 0, 0.05);
}

.filter__input__select__text {
  font-size: 14px;
  font-weight: 400;
}

.filter__input__select__icon {
  width: 12px;
  height: 12px;
}

.filter__input__select__icon path {
  fill: var(--secondary-color);
}

.filter__input__list {
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 100%;
  height: 0px;
  overflow: hidden;
  transform: translateY(100%);

  background: var(--primary-color);
  border-radius: 5px;
  box-shadow: 0px 2px 9px 0px rgba(0, 0, 0, 0.05);
}

.filter__input__list__container {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 16px 24px;
}

.filter__input__list.active {
  height: fit-content;
}

.filter__input__list__item {
  font-size: 14px;
  font-weight: 400;
  line-height: 20px;
  cursor: pointer;
}

@media (max-width: 1280px) {
  .input__container {
    flex-direction: column;
    align-items: flex-start;
    gap: 40px;
  }

  .search__input {
    width: 100%;
  }

  .country__container {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 768px) {
  .country__container {
    grid-template-columns: 1fr;
  }
}
</style>
