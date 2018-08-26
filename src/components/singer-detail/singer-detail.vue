<template>
  <transition name="slide">
    <div class="singer-detail-wrapper"></div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import {mapGetters} from 'vuex';
  import {getSingerDetail} from '../../api/singer';
  import {ERR_OK} from '../../api/config';
  import {createSong} from '../../common/js/song';

  export default {
    created() {
      this._getDetail();
    },
    data() {
      return {
        songs: []
      };
    },
    methods: {
      _getDetail: function () {
        if (!this.singer.id) {
          this.$router.push('/singer');
          return;
        }
        getSingerDetail(this.singer.id).then((res) => {
          if (res.code === ERR_OK) {
            this.songs = this._normalizeSongs(res.data.list);
            console.log(this.songs);
          }
        });
      },
      _normalizeSongs: function (list) {
        let ret = [];
        list.forEach((item) => {
          let {musicData} = item;
          ret.push(createSong(musicData));
        });
        return ret;
      }
    },
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
