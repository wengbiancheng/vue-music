<template>
  <div class="player">
    <div class="normal-player" v-show="fullScreen">播放器</div>
    <div class="mini-player" v-show="!fullScreen"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {mapGetters} from 'vuex';

  export default {
    computed: {
      ...mapGetters([
        'currentIndex',
        'fullScreen',
        'playing'
      ])
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl";
  .player
    .normal-player
      position: fixed
      left: 0
      right: 0
      top: 0
      bottom: 0
      z-index: 150
      background: $color-background
    .mini-player
      display: flex
      align-items: center
      position: fixed
      left: 0
      bottom: 0
      width: 100%
      height: 60px
      background: $color-highlight-background
</style>