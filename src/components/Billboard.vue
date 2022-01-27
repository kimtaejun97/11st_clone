<template>
  <div class="billboard">
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

</style>
