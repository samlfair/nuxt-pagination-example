<template>
  <div class="container">
    <ul>
      <li v-for="document in results" :key="document.id">
        {{ $prismic.asText(document.data.title) }}
      </li>
    </ul>
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
      return "/page/" + (+this.$route.params.num - 1);
    },
    nextPage() {
      return "/page/" + (+this.$route.params.num + 1);
    }
  }
};
</script>

<style></style>
