<template>
  <Wrapper :app="app">
    <main class="Container">
      <Cover
        v-if="app && app.cover && app.cover.value"
        :img="app.cover.value"
      />
      <div class="Articles">
        <ArticleCard
          v-for="article in articles"
          :key="article._id"
          :article="article"
        />
        <Pagination :total="total" :current="1" />
      </div>
    </main>
  </Wrapper>
</template>

<script>
import { getArticles } from 'api/article'
import { getApp } from 'api/app'

export default {
  async asyncData({ $config }) {
    const { articles, total } = await getArticles($config)
    const app = await getApp($config)
    return {
      articles,
      total,
      app,
    }
  },
}
</script>

<style scoped>
.Articles {
  padding: 24px 24px 40px 24px;
  margin: 0 auto;
}
@media (min-width: 600px) {
  .Articles {
    padding: 60px;
    max-width: 700px;
  }
}
</style>
