<template>
  <transition name="slide">
    <music-list :title="title" :bg-image="bgImage" :songs="songs"></music-list>
  </transition>
</template>

<script type="text/ecmascript-6">
  import {mapGetters} from 'vuex';
  import {getSingerDetail} from '../../api/singer';
  import {ERR_OK} from '../../api/config';
  import {createSong} from '../../common/js/song';
  import MusicList from '../../components/music-list/music-list';

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
      title: function () {
        return this.singer.name;
      },
      bgImage: function () {
        return this.singer.avatar;
      },
      ...mapGetters([
        'singer'
      ])
    },
    components: {
      MusicList
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .slide-enter-active, .slide-leave-active
    transition: all 0.3s

  .slide-enter, .slide-leave-to
    transform: translate3d(100%, 0, 0)
</style>
