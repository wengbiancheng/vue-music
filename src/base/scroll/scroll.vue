<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';

  export default {
    props: {
      probeType: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: false
      },
      listenScroll: {
        type: Boolean,
        default: false
      },
      data: {
        type: Array,
        default: null
      },
      refreshDelay: {
        type: Number,
        default: 20
      }
    },
    mounted() {
      setTimeout(() => {
        this._initScroll();
      }, this.refreshDelay);
    },
    methods: {
      _initScroll: function () {
        if (!this.$refs.wrapper) {
          return;
        }

        this.scroll = new BScroll(this.$refs.wrapper, {
          probeType: this.probeType,
          click: this.click
        });

        if (this.listenScroll) {
          this.scroll.on('scroll', (pos) => {
            this.$emit('scroll', pos);
          });
        }
      },
      disable: function () {
        this.scroll && this.scroll.disable();
      },
      enable: function () {
        this.scroll && this.scroll.enable();
      },
      refresh: function () {
        this.scroll && this.scroll.refresh();
      },
      scrollTo: function () {
        this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments);
      },
      scrollToElement: function () {
        this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments);
      }
    },
    watch: {
      data: function () {
        setTimeout(() => {
          this.scroll.refresh();
        }, this.refreshDelay);
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
</style>
