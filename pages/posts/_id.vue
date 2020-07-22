<template>
  <div class="container sm:px-16 px-6 pt-8 max-w-3xl">
    <div v-if="$fetchState.pending">Fetching post #{{$route.params.id}}...</div>
    <div v-else>
      <h1 class="text-4xl font-black pb-4 leading-tight">{{ item.title }}</h1>
      <p>
        <n-link to="/" class="text-gray-700 leading-tight hover:underline">Home</n-link>
      </p>
      <img
        v-if="item.hero_thumbnail"
        :src="'' + item.hero_thumbnail.url"
        :width="item.hero_thumbnail.width"
        :height="item.hero_thumbnail.height"
        class="pt-4 pb-4 rounded"
      />
      <h3 class="text-2xl pb-4 leading-tight">{{ item.introduction }}</h3>
      <span v-for="block in item.body" :key="block.id" id="streamfield">
        <div v-if="block.type == 'heading'">
          <h2 class="text-xl pb-4 leading-tight">{{ block.value }}</h2>
        </div>

        <div v-else-if="block.type == 'paragraph'">
          <p v-html="block.value"></p>
        </div>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      item: {
        hero_thumbnail: {
          url: ""
        }
      }
    };
  },
  head() {
    return {
      title: this.item.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: this.item.introduction
        },
        {
          hid: "og:image",
          name: "og:image",
          content: this.item.hero_thumbnail.url
        }
      ]
    };
  },
  async fetch() {
    this.item = await this.$http.$get(
      `https://jpl-poc.torchbox.com/api/v2/pages/${this.$route.params.id}/`
    );
  }
};
</script>

<style>
.page-enter-active,
.page-leave-active {
  transition-property: opacity;
  transition-timing-function: ease-in-out;
  transition-duration: 70ms;
}

.page-enter,
.page-leave-to {
  opacity: 0;
}
.athelas {
  font-family: "Source Serif Pro", athelas, georgia, serif;
}
#streamfield p {
  @apply mb-4 athelas text-lg;
}
</style>
