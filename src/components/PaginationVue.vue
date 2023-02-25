<template>
  <section
    :style="{
      '--pagination-item-width': width,
      '--pagination-item-height': height,
      '--color-active': colorActive,
      '--color-primary': colorPrimary,
      '--bg-button-disabled': bgButtonDisabled,
      '--bg-primary': bgPrimary,
      '--page-color-primary': pageColorPrimary,
    }"
    class="container"
  >
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
      class="page"
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
import { ref, defineProps } from "vue";
const props = defineProps({
  itemCount: {
    type: Number,
    required: true,
    default: 60,
  },
  itemCountPerPage: {
    type: Number,
    required: false,
    default: 5,
  },
  width: {
    type: String,
    required: false,
    default: "2rem",
  },
  height: {
    type: String,
    required: false,
    default: "2rem",
  },
  colorActive: {
    type: String,
    required: false,
    default: "#4200ff",
  },
  bgPrimary: {
    type: String,
    required: false,
    default: "white",
  },
  pageColorPrimary: {
    type: String,
    required: false,
    default: "black",
  },
  bgButtonDisabled: {
    type: String,
    required: false,
    default: "#919eab",
  },
  colorPrimary: {
    type: String,
    required: false,
    default: "#c4cdd5",
  },
  maxPageCount: {
    type: Number,
    required: false,
    default: 7,
    validator: (value) => {
      if (value < 6) {
        throw new Error("maxPageCount prop's value must be greater than 5");
      } else {
        return value;
      }
    },
  },
});

const activeIndex = ref(0);
let totalPage = Math.round(Math.ceil(props.itemCount / props.itemCountPerPage));

const displayPages = function () {
  return totalPage > props.maxPageCount
    ? activeIndex.value < 4
      ? [
          ...Array.from(
            { length: props.maxPageCount - 2 },
            (_, index) => index + 1
          ),
          "...",
          totalPage,
        ]
      : activeIndex.value > totalPage - 5
      ? [
          1,
          "...",
          ...Array.from(
            { length: props.maxPageCount - 2 },
            (_, index) => totalPage - index
          ).sort(),
        ]
      : [
          1,
          "...",
          activeIndex.value,
          ...Array.from(
            { length: props.maxPageCount - 5 },
            (_, index) => activeIndex.value + index + 1
          ),
          "...",
          totalPage,
        ]
    : totalPage;
};
</script>
<style>
:root {
  /* --color-active: #4200ff; */
  /* --color-primary: #c4cdd5; */
  --bg: #dfe3e8;
  /* --bg-primary: white;
  --bg-button-disabled: #919eab; */
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
  min-width: var(--pagination-item-width, 2rem);
  min-height: var(--pagination-item-height, 2rem);
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
  min-width: var(--pagination-item-width, 2rem);
  min-height: var(--pagination-item-height, 2rem);
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
  color: var(--page-color-primary);
  background-color: var(--bg-primary);
}

.page-active {
  border-color: var(--color-active);
  color: var(--color-active);
  border: solid 1px;
}
img {
  box-shadow: none;
}
</style>
