<template>
  <pagination-vue
    width="40px"
    :maxPageCount="9"
    :itemCountPerPage="2"
    :itemCount="30"
    @change="test($event)"
  >
  </pagination-vue>
</template>

<script>
import PaginationVue from "./components/PaginationVue.vue";

export default {
  name: "App",
  components: {
    PaginationVue,
  },
  methods: {
    test(page) {
      console.log(page + 1);
    },
  },
};
</script>

<style>
body * {
  box-sizing: border-box;
}
</style>
