<template>
  <main class="Container">
    <Cover img="https://as1.ftcdn.net/v2/jpg/03/45/18/76/1000_F_345187680_Eo4rKPDmdB6QTaGXFwU4NE5BaLlpGooL.jpg" />
    <div class="Articles">
      <ArticleCard v-for="article in articles" :key="article._id" :article="article" />
      <Pagination :total="total" :current="pageNumber" />
    </div>
  </main>
</template>

<script>
import { getArticles } from 'api/article'

export default {
  async asyncData({ $config, redirect, params }) {
    const pageNumber = Number(params.page)
    if (Number.isNaN(pageNumber)) return redirect(302, '/')

    const { articles, total } = await getArticles($config, { page: pageNumber })
    return {
      pageNumber,
      articles,
      total,
    }
  },
  data() {
    return {}
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