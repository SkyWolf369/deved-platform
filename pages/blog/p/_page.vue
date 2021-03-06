<template>
  <section class="Blog__Full-width">
    <header class="Blog__Full-width">
      <PageHero class="Category-hero">
        {{ $t('page_blog_title') }}
      </PageHero>
    </header>
    <main class="Vlt-container">
      <div class="Vlt-grid">
        <div class="Vlt-col" />
        <div v-if="routes" class="Vlt-col Vlt-col--2of3">
          <Breadcrumbs :routes="routes" />
        </div>
        <div class="Vlt-col" />
        <div class="Vlt-grid__separator" />
        <Card v-for="post in posts" :key="post.route" :post="post" />
        <div class="Vlt-grid__separator" />
        <div class="Vlt-col Vlt-center">
          <Pagination route="/blog" :page="page" :post-count="postCount" />
        </div>
      </div>
    </main>
  </section>
</template>

<script>
import Breadcrumbs from "~/components/Breadcrumbs"
import Card from "~/components/Card"
import PageHero from "~/components/PageHero"
import Pagination from "~/components/Pagination"
import config from "~/modules/config"

export default {
  validate ({ params: { page } }) {
    return /^\d+$/.test(page)
  },

  components: {
    Breadcrumbs,
    Card,
    PageHero,
    Pagination
  },

  async asyncData({ $content, app, error, params: { page } }) {
    try {
      page = parseInt(page, 10) || 1

      const postCount = (await $content(`blog/${app.i18n.locale}`)
        .where({ 'published': { '$ne': false } })
        .sortBy('published_at', 'desc')
        .only(['title'])
        .fetch()).length

      const postsQuery = $content(`blog/${app.i18n.locale}`)
        .where({ 'published': { '$ne': false } })
        .sortBy('published_at', 'desc')
        .skip(config.postsPerPage * (page  - 1))
        .limit(config.postsPerPage)

      const posts = await postsQuery.fetch()

      if (posts.length === 0) {
        error({ statusCode: 404, message: "Page not found" })
      }

      return {
        page,
        postCount,
        posts,
        routes: [
          { route: `/blog`, title: this.$t('page_blog_breadcrumb'), current: true },
        ]
      }
    } catch (e) {
      error(e)
    }
  },
}
</script>

<style scoped>
.Category-hero >>> .Blog-hero__content h3 .Vlt-badge {
  font-size: 21px;
  padding: 0 4px 0 0;
  line-height: 1;
}
</style>