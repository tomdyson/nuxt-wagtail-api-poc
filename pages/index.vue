<template>
  <div class="container sm:px-16 px-6 pt-8 max-w-2xl">
    <h1 class="text-6xl font-black pb-4">JPL News</h1>
    <p v-if="$fetchState.pending">Fetching posts...</p>
    <p v-else-if="$fetchState.error">Error while fetching posts: {{ $fetchState.error.message }}</p>
    <ul v-else>
      <li v-for="post of posts.items" :key="post.id" class="pb-4">
        <n-link
          :to="`/posts/${post.id}`"
          class="text-2xl font-bold text-gray-700 leading-tight hover:underline"
        >{{ post.title }}</n-link>
        <p class="pt-1 athelas text-lg">{{ post.introduction }}</p>
        <p class="pt-1 pb-4 athelas text-gray-600">{{ post.publication_date|datify }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: []
    };
  },
  head() {
    return {
      title: "JPL News",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "News from NASA / JPL"
        }
      ]
    };
  },
  async fetch() {
    this.posts = await this.$http.$get(
      "https://jpl-poc.torchbox.com/api/v2/pages/?type=news.NewsPage&fields=introduction,body,publication_date"
    );
  },
  filters: {
    datify: function(value) {
      if (value && value.length) {
        var d = new Date(Date.parse(value));
        var options = { year: "numeric", month: "long", day: "numeric" };
        return d.toLocaleDateString("en-us", options);
      }
    }
  }
};
</script>

<style scoped>
.athelas {
  font-family: "Source Serif Pro", athelas, georgia, serif;
}
</style>