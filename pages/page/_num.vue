<template>
  <div class="container">
    <ul>
      <li v-for="document in results">
        {{ $prismic.asText(document.data.title) }}
      </li>
    </ul>
    <nuxt-link
      v-if="+$route.params.num > 1"
      :to="'/page/' + (+$route.params.num - 1)"
      >« Prev Page</nuxt-link
    >
    <nuxt-link :to="'/page/' + (+$route.params.num + 1)">Next Page »</nuxt-link>
  </div>
</template>

<script>
export default {
  name: "Home",
  async asyncData({ $prismic, params, error }) {
    const { results } = await $prismic.api.query("", {
      pageSize: 20,
      page: params.num
    });
    if (results) {
      return { results };
    } else {
      error({ statusCode: 404, message: "Page not found" });
    }
  }
};
</script>

<style></style>
