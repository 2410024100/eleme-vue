<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image" alt="">
          <div @click="hide" class="icon-back">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}</span>
          </div>
          <div class="price">
            <span class="now"><span class="sign">￥</span>{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
        </div>
        <div class="cartcontrol-wrapper">
          <cartcontrol :food="food" @add="addFood"></cartcontrol>
        </div>
        <transition name="fade">
          <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count||food.count===0">加入购物车</div>
        </transition>
      </div>
    </div>
  </transition>
</template>

<script>
  import Vue from 'vue';
  import BScroll from 'better-scroll';
  import cartcontrol from 'components/cartcontrol/cartcontrol.vue';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false
      };
    },
    methods: {
      show() {
        this.showFlag = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
          ;
        });
      },
      hide() {
        this.showFlag = false;
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('add', event.target);
        Vue.set(this.food, 'count', 1);
      },
      addFood(target) {
        this.$emit('add', target);
      }
    },
    components: {
      cartcontrol
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "../../common/css/mixin";

  .food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #fff;
    transform: translate3d(0, 0, 0);
    &.move-enter-active, &.move-leave-active {
      transition: all 0.2s linear;
    }
    &.move-enter, &.move-leave-active {
      transform: translate3d(100%, 0, 0);
    }
    .food-content {
      .image-header {
        position: relative;
        width: 100%; /* 也可以 width: 100vw; height: 100vw，但兼容性不好 */
        height: 0;
        padding-top: 100%; /* 根据宽度计算, 所以显示的图片宽高相等（padding区域也显示图片）*/
        img {
          position: absolute;
          top: 0;
          left: 0;
          width: 100%; /* 撑满父容器 */
          height: 100%;
        }
        .icon-back {
          position: absolute;
          top: 10px;
          left: 10px;
          .icon-arrow_lift {
            display: block;
            padding: 10px;
            font-size: 20px;
            color: #fff;
          }
        }
      }
      .content {
        padding: 18px;
        .title {
          line-height: 14px;
          margin-bottom: 8px;
          font-size: 14px;
          font-weight: 700;
          color: rgb(7, 17, 27);
        }
        .detail {
          margin-bottom: 18px;
          line-height: 10px;
          font-size: 0;
          .sell-count, .rating {
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
          .sell-count {
            margin-right: 12px;
          }
        }
        .price {
          font-size: 0;
          line-height: 24px;
          height: 10px;
          .sign {
            font-size: 11px;
          }
          .now {
            margin-right: 8px;
            font-weight: 900;
            font-size: 16px;
            color: #fa3a12;
          }
          .old {
            text-decoration: line-through;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
        }
      }
      .cartcontrol-wrapper {
        position: absolute;
        right: 12px;
        bottom: 12px;
      }
      .buy {
        position: absolute;
        right: 18px;
        bottom: 18px;
        z-index: 10;
        height: 24px;
        line-height: 24px;
        padding: 0 12px;
        box-sizing: border-box;
        border-radius: 12px;
        font-size: 10px;
        color: #fff;
        background: rgb(0, 160, 220);
        opacity: 1;
        &.fade-enter-active, &.fade-leave-active {
          transition: all .2s;
        }
        &.fade-enter, &.fade-leave-to {
          opacity: 0;
          z-index: -1;
        }
      }
    }
  }

</style>
