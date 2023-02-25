<template>
  <section class="container">
    <button
      :disabled="activeIndex === 0 ? true : false"
      :class="{ 'button-disabled': activeIndex === 0 }"
      @click="activeIndex--"
      class="button-active"
    >
      &lt;
    </button>

    <component
      :class="[
        page == '...' ? 'dots' : 'page',
        { 'page-active': activeIndex + 1 === page },
      ]"
      class="page-disabled"
      @click="page === '...' ? undefined : (activeIndex = page - 1)"
      :key="page"
      v-for="page in displayPages()"
      :is="page == '...' ? 'div' : 'button'"
      >{{ page }}</component
    >

    <button
      :disabled="activeIndex + 1 === totalPage ? true : false"
      :class="{ 'button-disabled': activeIndex + 1 === totalPage }"
      @click="activeIndex++"
      class="button-active"
    >
      &gt;
    </button>
  </section>
</template>
<script setup>
import { ref } from "vue";
const activeIndex = ref(0);
const itemCount = 60;
const itemCountPerPage = 5;
const totalPage = Math.round(Math.floor(itemCount / itemCountPerPage));
const maxPageCount = 7;

const displayPages = function () {
  return totalPage > maxPageCount
    ? activeIndex.value < 4
      ? [1, 2, 3, 4, 5, "...", totalPage]
      : activeIndex.value > totalPage - 5
      ? [
          1,
          "...",
          totalPage - 4,
          totalPage - 3,
          totalPage - 2,
          totalPage - 1,
          totalPage,
        ]
      : [
          1,
          "...",
          activeIndex.value,
          activeIndex.value + 1,
          activeIndex.value + 2,
          "...",
          totalPage,
        ]
    : totalPage;
};
</script>
<style>
:root {
  --color-page-active: #4200ff;
  --color-primary: #c4cdd5;
  --bg: #dfe3e8;
  --bg-primary: white;
  --bg-button-disabled: #919eab;
}
.container {
  padding: 10rem;

  width: 100%;
  background-color: var(--bg);
  display: flex;
  gap: 8px;
  align-items: center;
  justify-content: center;
}
button {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 2rem;
  min-height: 2rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}
.dots {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 2rem;
  min-height: 2rem;
  border: none;
  border-radius: 4px;
}
button:not(.button-disabled):hover {
  transform: scale(1.05);
}
.button-active {
  background: var(--bg-primary);
  color: var(--color-primary);
  font-size: x-large;
}
.button-disabled {
  background: var(--bg-button-disabled);
  color: var(--color-primary);
  font-size: x-large;
  cursor: default;
}
.page {
  font-weight: bold;
  background: var(--bg-primary);
}
.page-active {
  border-color: var(--color-page-active);
  color: var(--color-page-active);
  border: solid 1px;
}
.page-disabled {
  background-color: var(--bg-primary);
}
img {
  box-shadow: none;
}
</style>
