<template>
  <ul class="blogIndex">
    <li v-for="item in $store.state.blogIndex.slice(0, 4)" :key="item.id">
      <a
        :href="`/blog/${item.id}`"
        :target="item.exSite ? '_blank' : null"
        :rel="item.exSite ? 'noopener' : null"
      >
        <img
          alt="背景"
          src="/blog/placeholder.png"
          :data-src="`/blog/${item.id}/cover.png`"
          width="600"
          height="300"
          class="bg js-lazy"
        />
        <img
          src="/blog/placeholder.png"
          :data-src="`/blog/${item.id}/cover.png`"
          :alt="`${item.title}のサムネイル画像`"
          width="600"
          height="300"
          class="js-lazy"
        />
        <h3>{{ item.title }}</h3>
        <div class="meta">
          <ul class="tags">
            <li v-for="tag in item.tags.slice(0, 2)" :key="tag">
              {{ tag }}
            </li>
          </ul>
          <time>{{ item.date }}</time>
        </div>
        <span v-if="item.exSite" class="c-exSite" :class="item.exSite">
          <SVG :symbol="`logo-${item.exSite}`" />
          <SVG symbol="open" />
        </span>
      </a>
    </li>
    <li class="more">
      <router-link to="/blog"><SVG symbol="next" />more</router-link>
    </li>
  </ul>
</template>

<script>
import { lazyImages } from "@/lib/lazyImages";

export default {
  name: "BlogIndex",
  mounted() {
    this.$nextTick(() => {
      lazyImages();
    });
  }
};
</script>

<style scoped lang="scss">
@use "@/style/common.scss" as *;

.blogIndex {
  margin-top: 2rem;
  display: grid;
  gap: 1.2rem;
  grid-template-columns: repeat(auto-fit, minmax(0rem, 1fr));
  @include max($MD) {
    grid-template-columns: repeat(5, minmax(32rem, 1fr));
    overflow: scroll;
    margin: #{-6 + 1.6}rem -6rem -6rem;
    padding: 6rem;
    z-index: 1;
    &::-webkit-scrollbar {
      display: none;
    }
  }
  > li {
    border: 1px solid color(main, 0.1);
    border-radius: 3.2rem 0.8rem;
    overflow: hidden;
    position: relative;
    background: rgba(#fff, 0.1);
    transition: $TRANSITION;
    will-change: transform;
    @media (prefers-color-scheme: light) {
      box-shadow: 0 1.6rem 6.4rem -2rem color(main, 0.3);
    }
    &:hover,
    &:active {
      transform: scale(1.02);
      .c-exSite {
        width: 12rem;
        height: 4.8rem;
      }
    }
    &:nth-child(4) {
      @include maxmin($XL, $MD) {
        display: none;
      }
    }
    &:nth-child(3) {
      @include maxmin(1280px, $MD) {
        display: none;
      }
    }
    &.more {
      display: none;
      background: none;
      @include max($MD) {
        display: block;
        box-shadow: none;
        border-radius: 2.4rem 0 0 2.4rem;
        border: 0.2rem solid color(main, 0.1);
        a {
          color: color(main, 0.8);
          padding: 0;
          display: flex;
          justify-content: center;
          align-items: center;
          flex-direction: column;
        }
        svg {
          width: 5.6rem;
          height: 5.6rem;
        }
        &:hover,
        &:active {
          background: color(main, 0.1);
        }
      }
    }
  }
  a {
    display: block;
    width: 100%;
    height: 100%;
    position: relative;
    padding-bottom: 5.6rem;
    background: color(base);
  }
  img.bg {
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0.4;
    -webkit-box-reflect: below 0;
    filter: blur(16px);
    transform: scale(1.02);
  }
  img {
    position: relative;
    width: 100%;
  }
  h3 {
    position: relative;
    margin: 1.2rem 1.6rem 0;
    font-size: 1em;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .meta {
    position: absolute;
    bottom: 1.6rem;
    left: 1.6rem;
    right: 1.6rem;
    display: grid;
    grid-template-columns: 1fr auto;
  }
  .tags {
    position: relative;
    display: flex;
    li {
      margin-right: 0.5em;
      background: color(main, 0.6);
      color: color(base);
      font-size: 1.2rem;
      height: 2.4rem;
      line-height: 2.1rem;
      letter-spacing: 0;
      padding: 0 1.2rem;
      border-radius: 1.2rem;
      max-width: 45%;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
  }
  time {
    font-size: 1.2rem;
    color: color(main, 0.6);
  }
}
</style>
