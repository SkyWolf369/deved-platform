<template>
  <div class="Vlt-table__pagination">
    <ul>
      <li class="Vlt-table__pagination__prev">
        <span v-if="page === first">Previous</span>
        <nuxt-link v-else :to="localePath(`${route}/p/${page - 1}`)">
          Previous
        </nuxt-link>
      </li>
      <li v-if="page-4 > first" class="Vlt-table__pagination__prev">
        <nuxt-link :to="localePath(`${route}/p/${page - 5}`)">
          ...
        </nuxt-link>
      </li>
      <template
        v-for="(p, index) in pages" 
      >
        <li
          v-if="
            p === page ||
              (p > (page-5) && p < page) ||
              (p < (page+5) && p > page)
          "
          :key="index"
          :class="{ 'Vlt-table__pagination__current': p === page }"
        >
          <span v-if="p === page">{{ p }}</span>
          <nuxt-link v-else :to="localePath(`${route}/p/${p}`)">
            {{ p }}
          </nuxt-link>
        </li>
      </template>
      <li v-if="page+4 < last" class="Vlt-table__pagination__prev">
        <nuxt-link :to="localePath(`${route}/p/${page + 5}`)">
          ...
        </nuxt-link>
      </li>
      <li class="Vlt-table__pagination__next">
        <span v-if="page === last">Next</span>
        <nuxt-link v-else :to="localePath(`${route}/p/${page + 1}`)">
          Next
        </nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
import config from "~/modules/config"

export default {
  props: {
    route: {
      type: String,
      required: true,
    },
    page: {
      type: Number,
      required: true,
    },
    postCount: {
      type: Number,
      required: true,
    },
  },

  data() {
    return {
      postsPerPage: config.postsPerPage
    }
  },

  computed: {
    pages() {
      return Array.from(Array(Math.ceil(this.postCount / this.postsPerPage)), (x, i) => i + 1)
    },
    first() {
      return this.pages[0]
    },
    last() {
      return this.pages[this.pages.length-1]
    }
  }
}
</script>