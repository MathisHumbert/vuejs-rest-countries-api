<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, RouterLink, useRouter } from 'vue-router';

const country = ref(null);

const router = useRouter();
const route = useRoute();

const fetchCountryData = async (id) => {
  const response = await fetch(
    `https://restcountries.com/v3.1/alpha?codes=${id}`
  );
  const data = await response.json();

  if (data.length) {
    country.value = data[0];
  }
};

const navigateToBordersCountry = (id) => {
  router.push(`/detail/${id}`);

  fetchCountryData(id);
};

onMounted(async () => {
  fetchCountryData(route.params.id);
});
</script>

<template>
  <section class="detail" v-if="country !== null">
    <RouterLink to="/" class="detail__back">
      <svg
        class="detail__back__icon"
        xmlns="http://www.w3.org/2000/svg"
        width="100%"
        height="100%"
        viewBox="0 0 20 20"
        fill="none"
      >
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M6.46447 4.10744L7.64298 5.28596L3.75389 9.17504L18.6031 9.17504L18.6031 10.825L3.75389 10.825L7.64298 14.714L6.46447 15.8926L0.57191 10L6.46447 4.10744Z"
          fill="white"
        />
      </svg>
      <p class="detail__back__text">Back</p>
    </RouterLink>
    <article class="detail__article">
      <img
        :src="country.flags.png"
        :alt="country.flags.alt"
        class="detail__img"
      />
      <div class="detail__container">
        <h2 class="detail__title">
          {{ country.name && country.name.official }}
        </h2>
        <div class="detail__content">
          <div class="detail__content__left">
            <div class="detail__content__container">
              <p class="detail__content__title">Native Name:</p>
              &nbsp;
              <p class="detail__content__text">
                {{
                  country.name.nativeName.eng
                    ? country.name.nativeName.eng.common
                    : country.name.common
                }}
              </p>
            </div>
            <div class="detail__content__container">
              <p class="detail__content__title">Population:</p>
              &nbsp;
              <p class="detail__content__text">{{ country.population }}</p>
            </div>
            <div class="detail__content__container">
              <p class="detail__content__title">Region:</p>
              &nbsp;
              <p class="detail__content__text">{{ country.region }}</p>
            </div>
            <div class="detail__content__container">
              <p class="detail__content__title">Sub Region:</p>
              &nbsp;
              <p class="detail__content__text">{{ country.subregion }}</p>
            </div>
            <div class="detail__content__container">
              <p class="detail__content__title">Capital:</p>
              &nbsp;
              <p class="detail__content__text">{{ country.capital[0] }}</p>
            </div>
          </div>
          <div class="detail__content__right">
            <div class="detail__content__container">
              <p class="detail__content__title">Top Level Domain:</p>
              &nbsp;
              <p class="detail__content__text">{{ country.tld[0] }}</p>
            </div>
            <div class="detail__content__container">
              <p class="detail__content__title">Currencies:</p>
              &nbsp;
              <p class="detail__content__text">
                {{ Object.values(country.currencies)[0].name }}
              </p>
            </div>
            <div class="detail__content__container">
              <p class="detail__content__title">Languages:</p>
              &nbsp;
              <p class="detail__content__text">
                {{ Object.values(country.languages).join(', ') }}
              </p>
            </div>
          </div>
        </div>
        <div class="detail__border">
          <p class="detail__border__text">Border Countries:</p>
          <div
            v-for="border in country.borders"
            @click="navigateToBordersCountry(border)"
            class="detail__border__card"
          >
            <p class="detail__border__card__text">{{ border }}</p>
          </div>
        </div>
      </div>
    </article>
  </section>
</template>

<style scoped>
.detail {
  padding: 80px;
}

.detail__back {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: 136px;
  height: 40px;
  background: var(--primary-color);
  border-radius: 6px;
  text-decoration: none;
}

.detail__back__icon {
  width: 20px;
  height: 20px;
}

.detail__back__text {
  font-size: 16px;
  font-weight: 300;
  color: var(--secondary-color);
}

.detail__article {
  display: flex;
  align-items: center;
  gap: 144px;
  margin-top: 90px;
}

.detail__img {
  width: 100%;
  object-fit: cover;
}

.detail__container {
  width: 100%;
}

.detail__title {
  margin-bottom: 32px;
  font-size: 32px;
  font-weight: 800;
}

.detail__content {
  display: flex;
  justify-content: space-between;
  margin-bottom: 70px;
}

.detail__content__left,
.detail__content__right {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.detail__content__container {
  display: flex;
}

.detail__content__title {
  font-size: 16px;
  font-weight: 600;
}

.detail__content__text {
  font-size: 16px;
  font-weight: 300;
}

.detail__border {
  display: flex;
  gap: 16px;
}

.detail__border__text {
  font-size: 16px;
  font-weight: 600;
}

.detail__border__card {
  width: 96px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 2px;
  background: var(--primary-color);
  box-shadow: 0px 0px 4px 1px rgba(0, 0, 0, 0.1);
}

.detail__border__card__text {
  font-size: 14px;
  font-weight: 300;
}
</style>
