<template>
  <article>
    <h1>{{ article.title }}</h1>
    <br />
    <p>{{ article.description }}</p>
    <br />
    <nuxt-content :document="article" />
    <br />
    <hr />
    <div class="more-info">
      <p>Post last updated: {{ formatDate(article.updatedAt) }}</p>
      <div>
        <span>Tags: </span>
        <span v-for="(tag, id) in article.tags" :key="id">
          <NuxtLink :to="`/blog/tags/${tags[tag].slug}`">
            <span>{{ tags[tag].name }} </span>
          </NuxtLink>
        </span>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content("articles", params.slug).fetch();

    const tagsList = await $content("tags")
      .only(["name", "slug"])
      .where({ name: { $containsAny: article.tags } })
      .fetch();

    const tags = Object.assign({}, ...tagsList.map((s) => ({ [s.name]: s })));

    const [prev, next] = await $content("articles")
      .only(["title", "slug"])
      .sortBy("createdAt", "asc")
      .surround(params.slug)
      .fetch();

    return {
      article,
      tags,
      prev,
      next,
    };
  },
  methods: {
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("en", options);
    },
  },
};
</script>

<style scoped>
article {
  width: 100%;
}

hr {
  height: 1px;
  border-width: 0;
  color: #e5e5e5;
  background-color: #e5e5e5;
}

.more-info {
  display: flex;
  justify-content: space-between;
  padding-top: 15px;
  flex-shrink: 0;
}
</style>