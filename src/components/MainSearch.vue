<template>
  <div
    class="flex flex-col text-sm bg-blue-300 border-r-0 font-light w-full sm:w-[400px] max-h-screen overflow-auto h-[500px] p-5 items-center space-y-3 rounded-sm shadow-lg"
  >
    <h1 class="text-lg">Pokemon Dex</h1>
    <SearchInput @on-search="onSearch" />
    <SearchResult
      :highlight="htmlHighlight"
      :results="searchResults"
      @on-select="onSelect"
    />
  </div>
</template>

<script setup lang="ts">
import { storeToRefs } from "pinia";
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "../store/store";
import SearchInput from "./SearchInput.vue";
import SearchResult from "./SearchResult.vue";

const store = useStore();
const router = useRouter();

const searchResults = ref([] as string[]);
const htmlHighlight = ref([] as string[]);

const { pokeNames } = storeToRefs(store);

const onSearch = (value: string) => {
  if (value && value.length > 0) {
    const { results, highlights } = store.findPokemonByName(value);
    if (results.length > 0) {
      searchResults.value = results.slice(0, 10).map((p: any) => p.target);
      htmlHighlight.value = highlights.slice(0, 10).map((h: any) => h ?? "");
    } else {
      searchResults.value = [];
      htmlHighlight.value = [];
    }
  } else {
    searchResults.value = pokeNames.value;
    htmlHighlight.value = [];
  }
};

function onSelect(name: string) {
  // router.push(`/pokemon/${name}`);
  window.dispatchEvent(new CustomEvent("pokemon-selected", { detail: name }));
}

onMounted(async () => {
  await store.getAllPokemon();
});
</script>
