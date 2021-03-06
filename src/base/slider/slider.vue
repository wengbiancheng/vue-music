<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
    <div class="dots">
      <span v-for="(item,index) in dots" class="dot" v-bind:key="item"
            :class="{active:currentPageIndex === index}"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {addClass} from '../../common/js/dom';
  import BScroll from 'better-scroll';

  export default {
    props: {
      loop: {
        type: Boolean,
        default: true
      },
      autoPlay: {
        type: Boolean,
        default: true
      },
      interval: {
        type: Number,
        default: 4000
      },
      click: {
        type: Boolean,
        default: true
      },
      threshold: {
        type: Number,
        default: 0.3
      },
      speed: {
        type: Number,
        default: 400
      }
    },
    data() {
      return {
        dots: [],
        currentPageIndex: 0
      };
    },
    mounted: function () {
      setTimeout(() => {
        this._initSlider();
        this._initDots();
        this._initScroll();

        if (this.autoPlay) {
          this._play();
        }

        window.addEventListener('resize', () => {
          if (!this.slider) {
            return;
          }
          this._initSlider(true);
          this.slider.refresh();
        });
      }, 20);
    },
    methods: {
      _initSlider: function (isResize) {
        this.children = this.$refs.sliderGroup.children;

        let width = 0;
        let sliderWidth = this.$refs.slider.clientWidth;
        for (let i = 0; i < this.children.length; i++) {
          let child = this.children[i];
          addClass(child, 'slider-item');
          child.style.width = sliderWidth + 'px';
          width += sliderWidth;
        }
        if (this.loop && !isResize) {
          width += 2 * sliderWidth;
        }
        this.$refs.sliderGroup.style.width = width + 'px';
      },
      _initDots: function () {
        this.dots = new Array(this.children.length);
      },
      _initScroll: function () {
        // 注意，better-scroll版本更新后，写法发生了变化，要懂得看文档！
        this.slider = new BScroll(this.$refs.slider, {
          scrollX: true,
          scrollY: false,
          momentum: false,
          snap: true,
          snapLoop: this.loop,
          snapThreshold: 0.3,
          snapSpeed: 400,
          client: true
        });

        this.slider.on('scrollEnd', () => {
          let pageIndex = this.slider.getCurrentPage().pageX;
          // TODO 可能新版本有所改动！而且也不懂为啥！后期有时间要弄弄
          if (this.loop) {
            pageIndex -= 1;
          }
          this.currentPageIndex = pageIndex;

          if (this.autoPlay) {
            clearTimeout(this.timer);
            this._play();
          }
        });
      },
      _play: function () {
        let pageIndex = this.currentPageIndex + 1;
        // TODO 可能新版本有所改动！而且也不懂为啥！后期有时间要弄弄
        if (this.loop) {
          pageIndex += 1;
        }
        this.timer = setTimeout(() => {
          this.slider.goToPage(pageIndex, 0, 400);
        }, this.interval);
      }
    },
    destroyed() {
      clearTimeout(this.timer);
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl"
  .slider
    min-height: 1px
    position: relative
    .slider-group
      position: relative
      overflow: hidden
      white-space: nowrap
      .slider-item
        float: left
        box-sizing: border-box
        overflow: hidden
        text-align: center
        a
          display: block
          width: 100%
          overflow: hidden
          text-decoration: none
        img
          display: block
          width: 100%
    .dots
      position: absolute
      right: 0
      left: 0
      bottom: 12px
      text-align: center
      font-size: 0
      .dot
        display: inline-block
        width: 8px
        height: 8px
        margin: 0px 4px
        border-radius: 50%
        background: $color-text-l
        &.active
          width: 20px
          border-radius: 5px
          background: $color-text-ll
</style>
