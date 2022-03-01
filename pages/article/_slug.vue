<template>
  <main class="Container">
    <article class="Article">
      <div class="Article_Header">
        <h1 class="Article_Title">{{ currentArticle.title }}</h1>
        <time :datetime="publishDateForAttr" class="Article_Date">{{
          publishDate
        }}</time>
      </div>
      <!-- eslint-disable-next-line vue/no-v-html -->
      <div class="Article_Body" v-html="currentArticle.body"></div>
    </article>
  </main>
</template>

<script>
import { mapGetters } from 'vuex'
import { formatDate } from 'utils/date'
import { htmlToText } from 'html-to-text'

export default {
  async asyncData({ $config, store, params, redirect }) {
    await store.dispatch('fetchApp', $config)
    await store.dispatch('fetchCurrentArticle', {
      ...$config,
      slug: params.slug,
    })
    if (!store.getters.currentArticle) return redirect(302, '/')
    return {}
  },
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.description,
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content: this.ogImage,
        },
      ],
    }
  },
  computed: {
    ...mapGetters(['app', 'currentArticle']),
    title() {
      if (this.currentArticle && this.currentArticle.title) {
        return this.currentArticle.title
      }
      return this.app && (this.app.name || this.app.uid || 'Blog')
    },
    description() {
      if (this.currentArticle && this.currentArticle.body) {
        return htmlToText(this.currentArticle.body, {
          selectors: [
            {
              selector: 'img',
              format: 'skip',
            },
          ],
        }).slice(0, 200)
      }
      return ''
    },
    publishDate() {
      return this.currentArticle && this.currentArticle._sys.createdAt
        ? formatDate(this.currentArticle._sys.createdAt)
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
.Article_Body >>> h1,
.Article_Body >>> h2,
.Article_Body >>> h3,
.Article_Body >>> h4,
.Article_Body >>> h5,
.Article_Body >>> h6 {
  padding: 0;
  margin: 40px 0 24px 0;
  line-height: 1.4;
}
.Article_Body >>> h1 {
  font-size: 2.4rem;
}
.Article_Body >>> h2 {
  font-size: 2.2rem;
}
.Article_Body >>> h3 {
  font-size: 2rem;
}
.Article_Body >>> h4 {
  font-size: 1.8rem;
}
.Article_Body >>> h5 {
  font-size: 1.6rem;
}
.Article_Body >>> h6 {
  font-size: 1.4rem;
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
.Article_Body >>> ul,
.Article_Body >>> ol {
  margin: 0;
  padding: 0 0 16px 40px;
}
.Article_Body >>> ul li,
.Article_Body >>> ol li {
  margin: 0 0 4px 0;
  padding: 0;
}
.Article_Body >>> ul li ul,
.Article_Body >>> ul li ol,
.Article_Body >>> ol li ol,
.Article_Body >>> ol li ul {
  padding: 0 0 0 20px;
}
.Article_Body >>> blockquote {
  border-left: 4px solid #ccc;
  padding: 0 0 0 40px;
  margin: 0 0 20px 0;
}
.Article_Body >>> pre {
  background: #333;
  color: #fff;
  border-radius: 4px;
  padding: 16px 20px;
  margin: 0 0 20px 0;
  font-size: 1.4rem;
  line-height: 1.6;
  overflow: auto;
  font-family: 'Segoe UI Emoji', 'Helvetica Neue', Arial,
    'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
}
.Article_Body >>> code {
  border: 1px solid #ddd;
  background: #f8f8f8;
  border-radius: 4px;
  padding: 2px 4px;
  margin: 0 4px;
  color: #e01d5a;
  font-size: 1.4rem;
  font-family: 'Segoe UI Emoji', 'Helvetica Neue', Arial,
    'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
}
.Article_Body >>> pre code {
  border: none;
  background: none;
  border-radius: 0;
  padding: 0;
  margin: 0;
  color: #fff;
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
