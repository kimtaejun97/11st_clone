<template>
  <div
    class="billboard"
    :style="`background-color: ${currentColor};`">
    <!-- Billboard Image -->
    <div class="inner">
      <div
        ref="swiper"
        class="swiper">
        <div class="swiper-wrapper">
          <div
            v-for="item in billboards"
            :key="item.name"
            class="swiper-slide">
            <img
              :data-src="item.src"
              class="swiper-lazy" />
            <div class="swiper-lazy-preloader"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Swiper from 'swiper/bundle'
import 'swiper/css/bundle'

export default {
  data() {
    return {
      billboards: [],
      currentColor: '',
      currentIndex: 0,
    }

  },
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      await this.getBillboards()  
      await this.$nextTick()
      this.swiper()

    },
    async getBillboards() {
      this.billboards = await this.$fetch({
        requestName: 'billboards'
      })
      console.log(this.billboards)
    },
    swiper() {
      this.swiper = new Swiper(this.$refs.swiper, {
        effect: 'fade',
        speed: 1000,
        autoplay: {
          delay: 3000
        },
        loop: true,
        preloadImages: false,
        lazy: {
          loadPrevNext: true,
          loadPrevNextAmount: 2
        },
        navigation: {
          prevEl: '.billboard .prev',
          nextEl: '.billboard .next'
        },
        on: {
          slideChange: swiper => {
            const { realIndex } = swiper
            this.currentIndex = realIndex
            this.currentColor = this.billboards[realIndex].color
          },
          autoplayStart: () => {
            this.isAutoplay = true
          },
          autoplayStop: () => {
            this.isAutoplay = false
          }
        }
      })
    }
  }
}
</script>

<style scoped lang=scss>
 .billboard {
    transition: background-color 1s;
  }
  .swiper {
    width: 1240px;
    height: 400px;
    .swiper-lazy-preloader {
      border-color: #F43242;
      border-top-color: transparent;
    }
  }
</style>
