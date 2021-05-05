<template>
  <div class="carousel">
    <!-- carousel item -->
    <VueSlickCarousel
      ref="carousel"
      v-bind="carousel.base"
      class="carousel__items"
      @init="onInitCarousel"
      @afterChange="onAfterCarousel"
    >
      <figure v-for="item in list" :key="item.caption" class="item">
        <img :src="item.image" class="item__image" />
        <figcaption class="item__caption">{{ item.caption }}</figcaption>
      </figure>
    </VueSlickCarousel>

    <!-- caption -->
    <div class="carousel__caption">
      <div class="caption">caption: {{ list[carousel.index].caption }}</div>
    </div>

    <!-- prev arrow -->
    <div class="carousel__prev">
      <button class="prev" @click="toPrevCarousel()">prev</button>
    </div>

    <!-- next arrow -->
    <div class="carousel__next">
      <button class="next" @click="toNextCarousel()">next</button>
    </div>

    <!-- paging list -->
    <div class="carousel__paging">
      <ul class="paging">
        <li v-for="(item, i) in list" :key="item.caption" class="paging__item">
          <button
            class="paging__button"
            :class="{ 'is-active': i === carousel.index }"
            @click="toGoToCarousel(i)"
          >
            {{ item.caption }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
// https://www.npmjs.com/package/vue-slick-carousel
import VueSlickCarousel from 'vue-slick-carousel'
import 'vue-slick-carousel/dist/vue-slick-carousel.css'

export default {
  components: { VueSlickCarousel },

  props: {
    // 画像リスト
    list: {
      type: Array,
      default: () => {
        return [
          {
            image: 'https://picsum.photos/707/500/',
            alt: 'dummy',
            caption: 'test1',
          },
          {
            image: 'https://picsum.photos/707/500/',
            alt: 'dummy',
            caption: 'test2',
          },
          {
            image: 'https://picsum.photos/707/500/',
            alt: 'dummy',
            caption: 'test3',
          },
          {
            image: 'https://picsum.photos/707/500/',
            alt: 'dummy',
            caption: 'test4',
          },
        ]
      },
    },
  },

  data() {
    return {
      /**
       * carouselのセッティングprops
       * - https://github.com/gs-shop/vue-slick-carousel/blob/master/docs/API.md#props
       *
       * 注1: 大きい画面サイズから指定していく(デスクトップファースト)
       * - ドキュメント通りにモバイル向け設定を書くとエラー起こすバグあり
       * - responsiveを空値にしておき、initイベント時に動的に設定を流し込む
       * - Issue: https://github.com/gs-shop/vue-slick-carousel/issues/94
       *
       * 注2: lazyLoadはちらつきが出るので指定しない
       */
      carousel: {
        // carouselの現在位置
        index: 0,

        // carouselの初期化判定（念の為）
        isInitialized: false,

        // ベースのセッティング
        base: {
          arrows: false, // 外出ししてるので既存機能はfalse
          autoplay: true,
          autoplaySpeed: 5000,
          speed: 1000,
          variableWidth: true,
          centerMode: true,
          responsive: [],
        },

        // 各種レスポンシブ対応セッティング
        responsive: [
          // mobileサイズ
          {
            breakpoint: 540,
            settings: {
              variableWidth: false,
              centerMode: false,
              slidesToShow: 1,
            },
          },
        ],
      },
    }
  },

  methods: {
    /**
     * carouselの初期化処理
     * @returns { void } - 初期化
     */
    onInitCarousel() {
      if (this.carousel.isInitialized === false) {
        this.carousel.base.responsive = this.carousel.responsive
        this.carousel.isInitialized = true
      }
    },

    /**
     * carouselのインデックス更新
     * 移動時のイベントをフックして間接的に現在のインデックスを更新
     * @returns { void }
     */
    onAfterCarousel(index) {
      this.carousel.index = index
    },

    /**
     * prev(前)のカルーセルへ移動
     * @returns { void }
     */
    toPrevCarousel() {
      this.$refs.carousel.prev()
    },

    /**
     * next(次)のカルーセルへ移動
     * @returns { void }
     */
    toNextCarousel() {
      this.$refs.carousel.next()
    },

    /**
     * 指定のカルーセルへ移動（ページイング用）
     * @returns { void }
     */
    toGoToCarousel(index) {
      this.$refs.carousel.goTo(index)
    },
  },
}
</script>

<style lang="scss" scoped>
.carousel {
  // .carousel__items

  &__items {
  }

  // .carousel__caption

  &__caption {
  }

  // .carousel__prev

  &__prev {
  }

  // .carousel__next

  &__next {
  }

  // .carousel__paging

  &__paging {
  }
}

.item {
  // .item__image

  &__image {
  }

  // .item__caption

  &__caption {
  }
}

.caption {
}

.prev {
}

.next {
}

.paging {
  // .paging__item

  &__item {
  }

  // .paging__button

  &__button {
  }
}
</style>
