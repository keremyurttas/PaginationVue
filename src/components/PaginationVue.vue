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
    class=""
  >
    <div class="container">
      <button
        :disabled="activeIndex === 0 ? true : false"
        @click="changePage('-')"
        class="button-active"
      >
        &lt;
      </button>

      <component
        v-for="page in displayPages"
        :class="[
          page == '...' ? 'dots' : 'page',
          { 'page-active': activeIndex + 1 === page },
        ]"
        class="page"
        @click="page === '...' ? undefined : changePage(page - 1)"
        :key="page"
        :is="page == '...' ? 'div' : 'button'"
        >{{ page }}</component
      >

      <button
        :disabled="activeIndex + 1 == totalPage ? true : false"
        @click="changePage('+')"
        class="button-active"
      >
        &gt;
      </button>
    </div>
  </section>
</template>
<script setup>
import { ref, watch, defineProps, computed, defineEmits } from "vue";
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
const emit = defineEmits(["change"]);

const activeIndex = ref(0);

const changePage = function (value) {
  typeof value === "number"
    ? (activeIndex.value = value)
    : value == "-"
    ? activeIndex.value--
    : activeIndex.value++;
  emit("change", activeIndex.value);
};
let totalPage = ref(
  Math.round(Math.ceil(props.itemCount / props.itemCountPerPage))
);
watch(props, () => {
  totalPage.value = Math.round(
    Math.ceil(props.itemCount / props.itemCountPerPage)
  );
});

const displayPages = computed(() => {
  return totalPage.value > props.maxPageCount
    ? activeIndex.value < props.maxPageCount / 2
      ? [
          ...Array.from(
            { length: props.maxPageCount - 2 },
            (_, index) => index + 1
          ),
          "...",
          totalPage.value,
        ]
      : activeIndex.value + 1 > totalPage.value - props.maxPageCount / 2
      ? [
          1,
          "...",
          ...Array.from(
            { length: props.maxPageCount - 2 },
            (_, index) => totalPage.value - index
          ).sort((a, b) => a - b),
        ]
      : props.maxPageCount % 2 === 0
      ? [
          1,
          "...",
          ...Array.from(
            { length: props.maxPageCount - 4 },
            (_, index) =>
              activeIndex.value +
              index -
              Math.ceil((props.maxPageCount - 7) / 2) +
              1
          ),
          "...",
          totalPage.value,
        ]
      : [
          1,
          "...",
          ...Array.from(
            { length: props.maxPageCount - 4 },
            (_, index) =>
              activeIndex.value +
              index -
              Math.ceil((props.maxPageCount - 7) / 2)
          ),
          "...",
          totalPage.value,
        ]
    : totalPage.value;
});
</script>
<style scoped>
section,
button {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}

.container {
  padding: 10rem;
  width: 100%;
  background-color: white;
  display: flex;
  gap: 8px;
  align-items: center;
  justify-content: center;
}
@media only screen and (max-width: 600px) {
  .container {
    padding: 0;
    justify-content: space-between;
    gap: 0;
  }
}
button {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: var(--pagination-item-width);
  min-height: var(--pagination-item-height);
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
  min-width: var(--pagination-item-width);
  min-height: var(--pagination-item-height);
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
button:disabled {
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
