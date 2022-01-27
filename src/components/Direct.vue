<template>
  <div class="direct">
    <div class="inner">
      <ul>
        <li
          v-for="direct in directs"
          :key="direct.name">
          <a :href="direct.href">
            <div>
              <img
                :src="direct.src"
                :alt="direct.name"
                width="90" />
              <div class="text">
                {{ direct.name }}
              </div>
            </div>
          </a>
        </li>
        <!-- 더 보기 버튼. -->
        <li>
          <div
            class="open-more"
            @click="onNav('LNB')">
            <div class="icon"></div>
            <div class="text">
              더보기
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      directs: [],
    }
  },
  created() {
    this.init ()
  },
  methods: {
    async init() {
      await this.getDirects()
    },
    async getDirects() {
      this.directs = await this.$fetch({
        requestName: 'directs'
      })
    },
    onNav(name) {
      this.$store.dispatch('navigation/onNav', name)
    }
  }
}
</script>

<style lang="scss" scoped>
  .direct {
    .inner {
      padding: 50px 0;
    }
    ul {
      display: flex;
      li {
        margin-right: 25px;
        &:last-child {
          margin-right: 0;
        }
        img {
          display: block;
          margin-bottom: 15px;
        }
        .text {
          font-size: 15px;
          text-align: center;
        }
        .open-more {
          cursor: pointer;
          .icon {
            width: 90px;
            height: 90px;
            margin-bottom: 15px;
            background-image: url("https://trusting-williams-8cacfb.netlify.app/images/main_2x.png");
            background-position: 0 0;
            background-size: 209px;
          }
        }
      }
    }
  }
</style>
