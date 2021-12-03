<template>
  <article v-if="article" class="Article">
    <NuxtLink :to="`/article/${article.slug}`" class="Article_Link">
      <time datetime="2021-09-12" class="Article_Date">{{formatDate(article._sys.createdAt)}}</time>
      <h2 class="Article_Title">{{article.title}}</h2>
    </NuxtLink>
  </article>
</template>

<script>
import { formatDate } from 'utils/date'

export default {
  props: {
    article: {
      type: Object,
      default: null
    }
  },
  computed: {
    coverImageSrc() {
      return (this.article.coverImage && this.article.coverImage.src) || 'http://placehold.jp/1076x664.png'
    },
    profileImageSrc() {
      return (this.article.author && this.article.author.profileImage && this.article.author.profileImage.src) || 'http://placehold.jp/32x32.png'
    },
    authorName() {
      return (this.article.author && this.article.author.fullName) || 'NO NAME'
    }
  },
  methods: {
    formatDate(dateStr) {
      return dateStr ? formatDate(dateStr) : ''
    }
  }
}
</script>

<style scoped>
.Article {
  border: 1px solid #e5e5e5;
  box-shadow: 0 2px 2px 0 rgba(0,0,0,.05);
  border-radius: 4px;
  margin: 0 0 16px 0;
}
.Article_Link {
  display: block;
  padding: 16px;
  line-height: 1.5;
  color: #333;
  text-decoration: none;
  border-radius: 4px;
  transition: background .2s;
  background: #fff;
}
.Article_Link:hover {
  background: #f8f8f8;
}
.Article_Link:active {
  background: none;
  transition: none;
}
.Article_Date {
  font-size: 1.2rem;
  color: #888;
  margin: 0 0 4px 0;
  vertical-align: top;
}
.Article_Title {
  font-size: 1.4rem;
  margin: 0;
  padding: 0;
}
</style>