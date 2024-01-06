<script setup>
import { ref } from 'vue';

const theme = ref('dark');

const updateTheme = () => {
  if (theme.value === 'dark') {
    theme.value = 'light';
    document.documentElement.classList.add('light');
  } else {
    theme.value = 'dark';
    document.documentElement.classList.remove('light');
  }
};
</script>

<template>
  <nav class="nav">
    <h1 class="nav__title">Were in the world?</h1>
    <div @click="updateTheme()" class="toggle">
      <svg
        v-if="theme === 'dark'"
        class="toggle__icon"
        width="100%"
        height="100%"
        viewBox="0 0 16 15"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M11.5532 10.815C7.66857 10.815 4.51929 7.92783 4.51929 4.36821C4.51929 3.0253 4.96679 1.78158 5.73143 0.75C2.69036 1.69515 0.5 4.33122 0.5 7.43807C0.5 11.3385 3.94929 14.5 8.20357 14.5C11.5929 14.5 14.4696 12.4932 15.5 9.70452C14.375 10.4048 13.0161 10.815 11.5532 10.815Z"
          fill="white"
        />
      </svg>
      <svg
        v-else
        class="toggle__icon"
        xmlns="http://www.w3.org/2000/svg"
        width="100%"
        height="100%"
        viewBox="0 0 20 20"
        fill="none"
      >
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M13.5532 13.815C9.66857 13.815 6.51929 10.9278 6.51929 7.36821C6.51929 6.0253 6.96679 4.78158 7.73143 3.75C4.69036 4.69515 2.5 7.33122 2.5 10.4381C2.5 14.3385 5.94929 17.5 10.2036 17.5C13.5929 17.5 16.4696 15.4932 17.5 12.7045C16.375 13.4048 15.0161 13.815 13.5532 13.815Z"
          fill="white"
          stroke="#111517"
          stroke-width="1.25"
        />
      </svg>
      <p class="toggle__text">Dark Mode</p>
    </div>
  </nav>
</template>

<style scoped>
.nav {
  display: flex;
  justify-content: space-between;
  widows: 100vw;
  padding: 24px 80px;
  background: var(--primary-color);
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.06);
}

.nav__title {
  font-size: 24px;
  font-weight: 800;
}

.toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.toggle__icon {
  width: 20px;
  height: 20px;
}

.toggle__text {
  font-size: 16px;
  font-weight: 600;
}

@media (max-width: 768px) {
  .nav__title {
    font-size: 14px;
  }

  .toggle__text {
    font-size: 12px;
  }

  .toggle__icon {
    width: 16px;
    height: 16px;
  }
}
</style>
