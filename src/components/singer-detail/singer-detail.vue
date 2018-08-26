<template>
  <transition name="slide">
    <div class="singer-detail-wrapper"></div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import {mapGetters} from 'vuex';

  export default {
    computed: {
      ...mapGetters([
        'singer'
      ])
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl";
  .singer-detail-wrapper
    position: fixed
    width: 100%
    height: 100%
    top: 0
    left: 0
    z-index: 100
    background: $color-background

  .slide-enter-active, .slide-leave-active
    transition: all 0.3s

  .slide-enter, .slide-leave-to
    transform: translate3d(100%, 0, 0)
</style>
