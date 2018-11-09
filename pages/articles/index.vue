<template>
  <section class="container">
    <h1>記事一覧</h1>
    <nuxt-link to="/" class="button--grey">トップへ戻る</nuxt-link>
    <div class="article-list">
      <ul>
        <li v-for="post in posts" :key="post.id">
          <nuxt-link :to="{ name: 'articles-slug', params: { slug: post.fields.slug }}">
            <div class="article-image">
              <img v-if="post.fields.thumbnailImage"
                  :src="post.fields.thumbnailImage.fields.file.url"
                  size="200px"
                  :alt="post.fields.thumbnailImage.fields.description">
            </div>
            <div class="title">
                <h2>{{ post.fields.title }}</h2>
                <p>公開日：{{ post.fields.publishDate }}</p>
            </div>
          </nuxt-link>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
  // contentfulの宣言
  import {createClient} from '~/plugins/contentful.js'
  // 設定情報の取得
  const client = createClient()

  export default {
    // 変数の宣言
    data () {
      return {
        posts: []
      }
    },
    // データの取得(非同期)
    asyncData ({ env }) {
      // contentfulより記事データを取得
      return client.getEntries({
        // 対象のブログID
        'content_type': env.CTF_BLOG_ID,
        // 最大取得件数
        'limit': 10,
      }).then(entries => {
        // 取得出来たのでindex.vueに返却
        return {
          posts: entries.items
        }
      }).catch(console.error)
    }
  }
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.article-list ul {
  padding: 0;
  margin: 0;
  list-style: none;
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  flex-wrap: wrap;
}

.article-list li {
  border: 1px solid #ccc;
  margin: 20px;
  padding: 20px;
}
</style>
