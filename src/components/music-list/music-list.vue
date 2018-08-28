<template>
  <div class="music-list">
    <div class="back">
      <i class="icon-back"></i>
    </div>
    <h1 class="title" v-html="title"></h1>
    <div class="bg-image" :style="bgStyle" ref="bgImage">
      <div class="filter" ref="filter"></div>
    </div>
    <div class="bg-layer" ref="layer"></div>
    <scroll class="list" :data="songs" :listen-scroll="listenScroll" :probe-type="probeType" ref="list"
            @scroll="scroll">
      <div class="song-list-wrapper">
        <song-list :songs="songs"></song-list>
      </div>
    </scroll>
  </div>
</template>

<script type="text/ecmascript-6">
  import Scroll from '../../base/scroll/scroll';
  import SongList from '../../base/song-list/song-list';
  import {prefixStyle} from '../../common/js/dom';

  const RESERVED_HEIGHT = 40;
  const transform = prefixStyle('transform');
  const backdrop = prefixStyle('backdrop-filter');

  export default {
    props: {
      title: {
        type: String,
        default: ''
      },
      bgImage: {
        type: String,
        default: ''
      },
      songs: {
        type: Array,
        default: function () {
          return [];
        }
      }
    },
    data() {
      return {
        listenScroll: true,
        probeType: 3,
        scrollY: 0
      };
    },
    created() {
    },
    watch: {
      scrollY: function (newY) {
        let translateY = Math.max(this.maxTranslateY, newY);
        let zIndex = 0;
        let scale = 1.0;
        let blur = 0;
        const percent = Math.abs(newY / this.imageHeight);
        if (newY > 0) {
          // 下拉
          scale = 1.0 + percent;
          zIndex = 10;
        } else {
          blur = Math.min(20, percent * 20);
        }
        this.$refs.layer.style[transform] = `translate3d(0,${translateY}px,0)`;
        if (this.maxTranslateY > newY) {
          zIndex = 10;
          this.$refs.bgImage.style.paddingTop = 0;
          this.$refs.bgImage.style.height = `${RESERVED_HEIGHT}px`;
        } else {
          this.$refs.bgImage.style.paddingTop = '70%';
          this.$refs.bgImage.style.height = 0;
        }
        this.$refs.bgImage.style[transform] = `scale(${scale})`;
        this.$refs.filter.style[backdrop] = `blur(${blur}px)`;
        this.$refs.bgImage.style.zIndex = zIndex;
      }
    },
    computed: {
      bgStyle: function () {
        return `background-image:url(${this.bgImage})`;
      }
    },
    mounted() {
      this.imageHeight = this.$refs.bgImage.clientHeight;
      this.maxTranslateY = -this.imageHeight + RESERVED_HEIGHT;
      this.$refs.list.$el.style.top = `${this.imageHeight}px`;
    },
    methods: {
      scroll: function (pos) {
        this.scrollY = pos.y;
      }
    },
    components: {
      Scroll,
      SongList
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl";
  @import "../../common/stylus/mixin.styl";

  .music-list
    position: fixed
    top: 0
    left: 0
    bottom: 0
    right: 0
    z-index: 100
    background: $color-background
    .back
      position: absolute
      top: 0
      left: 6px
      z-index: 50
      .icon-back
        display: block
        padding: 10px
        font-size: $font-size-large-x
        color: $color-theme
    .title
      position: absolute
      top: 0
      left: 10%
      z-index: 40
      width: 80%
      no-wrap()
      text-align: center
      line-height: 40px
      font-size: $font-size-large
      color: $color-text
    .bg-image
      position: relative
      width: 100%
      height: 0px
      padding-top: 70%
      transform-origin: top
      background-size: cover
      .filter
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
        background: rgba(7, 17, 27, 0.4)
    .bg-layer
      position: relative
      height: 100%
      background: $color-background
    .list
      position: fixed
      top: 0px
      bottom: 0px
      width: 100%
      background: $color-background
      .song-list-wrapper
        padding: 20px 30px
</style>
