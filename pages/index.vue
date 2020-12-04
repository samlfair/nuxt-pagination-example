<template>
  <div class="container">
    <ul>
      <li v-for="document in results" :key="document.id">
        {{ $prismic.asText(document.data.title) }}
      </li>
    </ul>
    <nuxt-link to="/page/2">Next Page »</nuxt-link>
  </div>
</template>

<script>
export default {
  name: "Home",
  async asyncData({ $prismic, params, error }) {
    const { results } = await $prismic.api.query("");
    if (results) {
      return { results };
    } else {
      error({ statusCode: 404, message: "Page not found" });
    }
  }
};
</script>

<style></style>
