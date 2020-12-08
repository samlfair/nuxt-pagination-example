<template>
  <div class="container">
    <ol :start="start">
      <li v-for="document in results" :key="document.id">
        {{ $prismic.asText(document.data.title) }}
      </li>
    </ol>
    <nuxt-link v-if="notFirstPage" :to="prevPage">« Prev Page</nuxt-link>
    <nuxt-link :to="nextPage">Next Page »</nuxt-link>
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
  },
  computed: {
    notFirstPage() {
      return +this.$route.params.num > 1;
    },
    prevPage() {
      if (this.$route.params.num === "2") return "/";
      return "/page/" + (+this.$route.params.num - 1);
    },
    nextPage() {
      return "/page/" + (+this.$route.params.num + 1);
    },
    start() {
      return (+this.$route.params.num - 1) * 20 + 1;
    }
  }
};
</script>

<style></style>
