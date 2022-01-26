<template>
  <nav
    v-if="done"
    class="show">
    <div class="user">
      <a>로그인</a>
      <div class="flex-space"></div>
      <div class="close-nav"></div>
    </div>
    <div class="container">
      <!-- 카테고리 -->
      <div class="group categories">
        <h3 class="group__title">
          카테고리
        </h3>
        <ul class="group__list">
          <li
            v-for="(item1, index) in navigations.categories.list"
            :key="item1.name">
            <div class="category-icon"></div>
            {{ item1.name }}
            <ul class="depth">
              <li
                v-for="item2 in item1.list"
                :key="item2.name">
                <a :href="item2.href">
                  {{ item2.name }}
                </a>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
  export default {
    data() {
      return {
        navigations: {},
        done: false
      }
    },
    created() {
      this.init()
    },
    methods: {
      async init() {
        this.navigations = await this.$fetch({
          requestName: 'navigations'
        })
        this.done = true
      }
    }
  }
</script>

<style scope lang="scss">
  nav {
    width: 300px;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 99;
    background-color: #f6f6f6;
    transition:  transform, .4s;
    transform: translateX(-300px);
    &.show {
      transform: translateX(0);
    }
    .user {
      height: 70px;
      padding: 0 25px;
      background-color: #fff;
      display: flex;
      align-items: center;
      a {
        font-size: 20px;
        font-weight: 700;
        text-decoration: none;
        &:hover {
          text-decoration: underline;
        }
      }
      .flex-space {
        flex-grow: 1;
      }
      .close-nav {
        width: 36px;
        height: 36px;
        background-image: url("https://trusting-williams-8cacfb.netlify.app/images/globals_2x.png");
        background-position: -261px -203px;
        background-size: 363px;
        cursor: pointer;
      }
    }
    .container {
      height: calc(100% - 164px); // user height 70px + exhibitions height 94px
      padding: 10px 0;
      box-sizing: border-box;
      overflow-y: auto;
      a {
        color: #333;
      }
      .group {
        background-color: #fff;
        margin-bottom: 10px;
        &:last-child {
          margin-bottom: 0;
        }
        &__tile {
          padding: 14px 25px;
          font-size: 17px;
          font-weight: 700;
          position: relative;
        }
        &__list {
          li {
            display: flex;
            align-items: center;
            box-sizing: border-box;
            cursor: pointer;
            a {
              display: flex;
              align-items: center;
              width: 100%;
              height: 100%;
              box-sizing: border-box;
            }
          }
        }
      }
    }
  }
</style>
