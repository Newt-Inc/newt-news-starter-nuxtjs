<template>
  <Wrapper :app="app" :use-h1="false">
    <main class="Container">
      <article class="Article">
        <div class="Article_Header">
          <h1 class="Article_Title">{{ article.title }}</h1>
          <time :datetime="publishDateForAttr" class="Article_Date">{{
            publishDate
          }}</time>
        </div>
        <!-- eslint-disable-next-line vue/no-v-html -->
        <div class="Article_Body" v-html="article.body"></div>
      </article>
    </main>
  </Wrapper>
</template>

<script>
import { getArticleBySlug } from 'api/article'
import { getApp } from 'api/app'
import { formatDate } from 'utils/date'

export default {
  async asyncData({ $config, params }) {
    const article = await getArticleBySlug($config, params.slug)
    const app = await getApp($config)
    return {
      article,
      app,
    }
  },
  computed: {
    publishDate() {
      return this.article._sys.createdAt
        ? formatDate(this.article._sys.createdAt)
        : ''
    },
    publishDateForAttr() {
      return this.publishDate.replace(/\//g, '-')
    },
  },
}
</script>

<style scoped>
.Article {
  padding: 24px;
  margin: 0 auto;
}
.Article_Header {
  margin: 0 0 24px 0;
}
.Article_Title {
  font-size: 2.4rem;
  line-height: 1.5;
  font-weight: bold;
  margin: 0;
  padding: 0;
}
.Article_Date {
  color: #888;
  margin: 8px 0 0 0;
}
.Article_Body >>> p {
  margin: 0 0 24px 0;
}
.Article_Body >>> img {
  max-width: 100%;
  height: auto;
  margin: 32px auto;
  display: block;
}
@media (min-width: 600px) {
  .Article {
    max-width: 700px;
    padding: 60px;
  }
  .Article_Header {
    margin: 0 0 48px 0;
  }
}
</style>
