<template>
  <article v-if="article" class="Article">
    <NuxtLink :to="`/article/${article.slug}`" class="Article_Link">
      <div class="Article_Eyecatch">
        <img :src="coverImageSrc" alt="" width="1076" height="664" />
      </div>
      <div class="Article_Inner">
        <h2 class="Article_Title">{{article.title}}</h2>
        <div class="Article_Content">
          <div class="Article_Data">
            <div class="Article_Avatar">
              <img :src="profileImageSrc" alt="" width="32" height="32" />
            </div>
            <div class="Article_DataText">
              <div class="Article_AuthorName">{{authorName}}</div>
              <time datetime="2021-09-12" class="Article_Date">{{formatDate(article._sys.createdAt)}}</time>
            </div>
          </div>
        </div>
      </div>
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
  width: 100%;
}
.Article_Link {
  display: flex;
  padding: 0;
  line-height: 1.5;
  color: #333;
  text-decoration: none;
  border-radius: 3px;
  transition: background .2s;
  background: #fff;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
}
.Article_Link:hover {
  background: #f8f8f8;
}
.Article_Link:active {
  background: none;
  transition: none;
}
.Article_Eyecatch {
  width: 100%;
  position: relative;
}
.Article_Eyecatch::before {
  content: "";
  padding-top: 54%;
  display: block;
}
.Article_EyecatchInner {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
}
.Article_Eyecatch > img {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  object-fit: cover;
  font-family: 'object-fit: cover'; /* IE11 */
  width: 100%;
  height: 100%;
}
.Article_Inner {
  padding: 16px;
  flex: 1 auto;
  display: flex;
  flex-direction: column;
}
.Article_Title {
  font-size: 1.4rem;
  margin: 0 0 12px 0;
  padding: 0;
}
.Article_Content {
  margin: auto 0 0 0;
  display: flex;
  align-items: center;
}
.Article_Data {
  display: flex;
  align-items: center;
}
.Article_Avatar {
  width: 32px;
  height: 32px;
  border-radius: 16px;
  overflow: hidden;
  margin: 0 12px 0 0;
  flex-shrink: 0;
}
.Article_Avatar img {
  width: 32px;
  height: 32px;
  object-fit: cover;
  font-family: 'object-fit: cover'; /* IE11 */
}
.Article_AuthorName {
  font-size: 1.2rem;
  font-weight: bold;
}
.Article_Date {
  font-size: 1.2rem;
  color: #888;
}
.Article_Fav {
  margin: 0 8px 0 auto;
  display: flex;
  align-items: center;
}
.Article_Fav > em {
  font-size: 1.2rem;
  font-style: normal;
  margin: 0 0 0 4px;
  color: #888;
}
@media (min-width: 600px) {
  .Article {
    margin: 0 12px 24px 0;
    width: calc(50% - 14px);
  }
  .Article:nth-child(2n) {
    margin: 0 0 24px auto;
  }
}
@media (min-width: 960px) {
  .Article {
    margin: 0 24px 24px 0;
    width: calc(33.333% - 18px);
  }
  .Article:nth-child(2n) {
    margin: 0 24px 24px 0;
  }
  .Article:nth-child(3n) {
    margin: 0 0 24px 0;
  }
}
</style>