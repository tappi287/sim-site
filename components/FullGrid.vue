<template>
  <div class="xs-text-6 md-text-5">
    <div v-if="posts[0]" :style="`margin-top:var(--nav-height);`" class="r browse full-height">
      <div v-for="(p, i) in posts" :key="i" :style="`height:calc(100vh - var(--nav-height);`"
           class="xs-border xs-p2 full-item">
        <div v-if="p.thumbnail"
             class="item xs-block xs-full-height xs-flex xs-relative xs-flex-align-start xs-flex-justify-end xs-text-left">
          <div class="xs-text-left xs-flex xs-full-height xs-flex-justify-end xs-flex-align-end xs-width-auto">
            <nuxt-link :to="p.path" class="full-bg-link">{{ p.title }}</nuxt-link>
          </div>
          <nuxt-link :to="p.path">
            <img :key="p.thumbnail" v-lazy="p.thumbnail" class="full-bg-image"/>
            <div v-if="!p.thumbnail" class="full-bg-color"></div>
          </nuxt-link>
        </div>
        <div v-else
             class="item item-txt xs-block xs-full-height xs-flex xs-relative xs-flex-align-center xs-flex-justify-center xs-text-center">
          <nuxt-link :to="p.path" class="nobg-link">{{ p.title }}</nuxt-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    posts: {
      type: Array,
    },
  },
  computed: {
    hasPagination() {
      return this.$store.state.pagination.active || false;
    },
  },
  created() {
    console.log('Post data', this.posts)
  }
};
</script>

<style>
img[lazy="loading"] {
  opacity: 0;
  transition: 0.8s all;
  transition-delay: 0.8s;
}

img[lazy="loaded"] {
  opacity: 1;
  transition: 0.8s all;
  transition-delay: 0.8s;
}

.nobg-link {
  font-size: calc(1.4rem + 2vw);
}

.full-bg-link {
  z-index: 2;
  padding: 1.2rem;
  transition: 0.8s all;
}

.item-txt {
  border: 1px solid rgba(0, 0, 0, 0.2);
  background: rgb(255, 255, 255);
  background: radial-gradient(
      circle,
      rgba(255, 255, 255, 1) 19%,
      rgba(247, 247, 247, 1) 100%
  );
}

.full-bg-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  object-fit: cover;
  object-position: 50% 50%;
  width: 100%;
  height: 100%;
  transition: 0.4s all;
  border: 1px solid rgba(0, 0, 0, 0.2);
}

.item:hover .full-bg-image,
.item:hover .full-bg-color {
  opacity: 0.8;
  transition: 0.4s all;
}

.item .full-bg-link {
  background: #fff;
  transition: 0.8s all;
  border-top: 1px solid rgba(0, 0, 0, 0.2);
  border-right: 1px solid rgba(0, 0, 0, 0.2);
}

.full-bg-link h2 {
  margin: 0;
}
</style>
