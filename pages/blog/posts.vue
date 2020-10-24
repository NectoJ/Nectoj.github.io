<template>
  <div>
    <h1>Blog Posts</h1>
    <p>You will find all of my blog posts here.</p>
    <ul>
      <div class="posts">
        <li v-for="article of articles" :key="article.slug">
          <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
            <div>
              <h4>{{ article.title }}</h4>
              <p class="description">{{ article.description }}</p>
              <p class="creationDate">{{ formatDate(article.createdAt) }}</p>
            </div>
          </NuxtLink>
        </li>
      </div>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content("articles", params.slug)
      .only(["title", "description", "img", "slug", "author", "createdAt"])
      .sortBy("createdAt", "asc")
      .fetch();

    return {
      articles,
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
.posts {
  padding-top: 2em;
}

.posts > li > a > div {
  padding-bottom: 1em;
}

.description {
  font-size: 0.9em;
}

.creationDate {
  font-size: 0.65em;
}
</style>