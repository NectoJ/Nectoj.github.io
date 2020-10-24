<template>
  <div>
    <NuxtLink to="/"><Logo /></NuxtLink>
    <h1>
      {{ tag.name }}
    </h1>
    <p>{{ tag.description }}</p>

    <nuxt-content :document="tag" />
    <br/>
    <NuxtLink to="/"
      ><p>Return to all posts</p></NuxtLink
    >
    <br/>
    <h3>Articles tagged {{ tag.name }}:</h3>
    <ul>
      <li v-for="article in articles" :key="article.slug">
        <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
          <img v-if="article.img" :src="article.img" :alt="article.alt" />

          <h2>{{ article.title }}</h2>
          <p>{{ article.description }}</p>
          <p>
            {{ formatDate(article.updatedAt) }}
          </p>
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const tags = await $content("tags")
      .where({ slug: { $contains: params.tag } })
      .limit(1)
      .fetch();

    const tag = tags.length > 0 ? tags[0] : {};

    const articles = await $content("articles", params.slug)
      .where({ tags: { $contains: tag.name } })
      .sortBy("createdAt", "asc")
      .fetch();

    return {
      articles,
      tag,
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

<style>
</style>