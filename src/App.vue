<template>
  <v-app>
    <v-container>
      <text-tags :tags="tags" alignment="left"></text-tags>
      <text-tags :tags="tags" alignment="justify"></text-tags>
      <div class="half-width">
        <text-tags :tags="tags" alignment="left"></text-tags>
        <text-tags :tags="tags" alignment="justify"></text-tags>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import TextTags from './components/TextTags.vue';

export default {
  name: 'App',
  components: {
    TextTags
  },
  data() {
    return {
      tags: [
  { text: "текст1" },
  { text: "текст2", icon: "mdi-star" },
  { text: "текст3" },
  { text: "текст4" },
  { text: "текст5", icon: "mdi-check" }
]
    };
  }
};
</script>

<style>
.half-width {
  display: flex;
  justify-content: space-between;
}

.half-width > * {
  width: 48%;
}
</style>
