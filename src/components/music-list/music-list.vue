<template>
  <div class="music-list">
    <div class="back">
      <i class="icon-back"></i>
    </div>
    <h1 class="title" v-html="title"></h1>
    <div class="bg-image" :style="bgStyle" ref="bgImage">
      <div class="filter" ref="filter"></div>
    </div>
    <scroll class="list" :data="songs" :listen-scroll="listenScroll" :probe-type="probeType" ref="list">
      <div class="song-list-wrapper">
        <song-list :songs="songs"></song-list>
      </div>
    </scroll>
  </div>
</template>

<script type="text/ecmascript-6">
  import Scroll from '../../base/scroll/scroll';
  import SongList from '../../base/song-list/song-list';

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
        probeType: 3
      };
    },
    created() {
    },
    computed: {
      bgStyle: function () {
        return `background-image:url(${this.bgImage})`;
      }
    },
    mounted() {
      this.imageHeight = this.$refs.bgImage.clientHeight;
      this.$refs.list.$el.style.top = `${this.imageHeight}px`;
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
    .list
      position: fixed
      top: 0px
      bottom: 0px
      width: 100%
      background: $color-background
      overflow: hidden
      .song-list-wrapper
        padding: 20px 30px
</style>
