<template>
  <div class="recommend">
    <scroll ref="scroll" class="recommend-content" :data="discList">
      <div>
        <div class="slider-wrapper" v-if="recommends.length">
          <slider>
            <div v-for="(item,index) in recommends" v-bind:key="index">
              <a :href="item.linkUrl">
                <img :src="item.picUrl" @load="loadImage">
              </a>
            </div>
          </slider>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">热门歌单推荐</h1>
          <ul>
            <li class="item" v-for="(item,index) in discList" v-bind:key="index">
              <div class="icon">
                <img :src="item.imgurl" width="60" height="60">
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </scroll>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend, getDiscList} from '../../api/recommend';
  import {ERR_OK} from '../../api/config';
  import Slider from '../../base/slider/slider';
  import Scroll from '../../base/scroll/scroll';

  export default {
    data() {
      return {
        recommends: [],
        discList: []
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
          this.discList = res.data.list;
        });
      },
      loadImage: function () {
        if (!this.checkloaded) {
          this.checkloaded = true;
          setTimeout(() => {
            // TODO 这种用法第一次见！要注意
            this.$refs.scroll.refresh();
          }, 20);
        }
      }
    },
    components: {
      Slider,
      Scroll
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
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            flex: 1
            display: flex
            flex-direction: column
            justify-content: center
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
</style>
