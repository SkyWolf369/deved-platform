<template>
  <div class="Vlt-col Vlt-col--1of2">
    <div class="Vlt-card Vlt-bg-white">
      <a v-if="post.redirect" :href="post.redirect" :title="post.title" class="Vlt-card__image" :style="`background-image: url('${post.thumbnail}'); background-size: cover; background-position: center; height: 200px`" />
      <NLink v-else :to="localePath(post.route, post.locale)" :title="post.title" class="Vlt-card__image" :style="`background-image: url('${post.thumbnail}'); background-size: cover; background-position: center; height: 200px`" />
      <div class="Vlt-card__header">
        <a v-if="post.redirect" :href="post.redirect" :title="post.title">
          <h4 class="Blog-truncate">
            {{ post.title }}
          </h4>
        </a>
        <NLink v-else :to="localePath(post.route, post.locale)" :title="post.title">
          <h4 class="Blog-truncate">
            {{ post.title }}
          </h4>
        </NLink>
      </div>
      <div class="Vlt-card__content Vlt-grey-dark">
        <small v-if="post.published_at">
          Published
          <strong>{{
            post.published_at | moment("dddd, MMMM Do YYYY")
          }}</strong>
        </small>
        <small v-if="post.author">
          by
          <Author :author="post.author" type="name" />
        </small>
      </div>
      <div class="Vlt-card__footer Vlt-card__footer--short Vlt-left">
        <Tags :tags="post.tags" />
      </div>
      <div class="Vlt-card__footer Vlt-card__footer--short Vlt-card__footer--noborder Vlt-right">
        <Category :category="post.categoryObject" class="Category--border" />
      </div>
    </div>
  </div>
</template>

<script>
import Tags from "~/components/Tags"
import Category from "~/components/Category"
import Author from "~/components/Author"

export default {
  components: {
    Category,
    Author,
    Tags,
  },

  props: {
    post: {
      type: Object,
      required: true,
    },
  }
}
</script>

<style scoped>
.Vlt-card__footer {
  line-height: 30px;
}

.Vlt-card__footer + .Vlt-card__footer--noborder {
  margin-top: -37px;
}

.Vlt-card__footer >>> .Vlt-badge-group {
  width: 75%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.Vlt-card__corner {
  padding: 0;
}

.Vlt-card__content small >>> a.Author__Name {
  font-size: 1.2rem;
  font-weight: 600;
}

.Vlt-card__content small >>> a.Author__Name:visited {
  color: #202020;
}

.Vlt-card__header {
  padding-bottom: 8px;
}

.Category--border {
  border: 1px solid rgba(19,20,21,.2);
}

@media only screen and (min-width: 575px) {
  .Vlt-card__header h4.Blog-truncate {
    display: block;
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
}
</style>