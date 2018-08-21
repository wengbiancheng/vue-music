<template>
  <div class="recommend">
    <div class="recommend-content">
      <div class="slider-wrapper" v-if="recommends.length">
        <slider>
          <div v-for="(item,index) in recommends" v-bind:key="index">
            <a :href="item.linkUrl">
              <img :src="item.picUrl">
            </a>
          </div>
        </slider>
      </div>
      <div class="recommend-list">
        <h1 class="list-title">热门歌单推荐</h1>
        <ul></ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend, getDiscList} from '../../api/recommend';
  import {ERR_OK} from '../../api/config';
  import Slider from '../../base/slider/slider';

  export default {
    data() {
      return {
        recommends: []
      };
    },
    created() {
      this._getRecommend();
      this._getDiscList();
    },
    methods: {
      _getRecommend: function () {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider;
          }
        });
      },
      _getDiscList: function () {
        getDiscList().then((res) => {
          console.log(res);
        });
      }
    },
    components: {
      Slider
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl";
  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0px
    .recommend-content
      height: 100%
      overflow: hidden
      .slide-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
</style>
