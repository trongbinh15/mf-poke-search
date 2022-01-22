<template>
  <MainSearch />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import MainSearch from "./components/MainSearch.vue";

export default defineComponent({
  name: "App",
  components: {
    MainSearch,
},
});
</script>

<style>
* {
  font-family: "SuperLegendBoy";
}
</style>
