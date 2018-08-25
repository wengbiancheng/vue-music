<template>
  <scroll class="listview" ref="listview" :data="data" @scroll="scroll" :listen-scroll="listenScroll" :probe-type="probeType">
    <ul>
      <li v-for="(group,index) in data" v-bind:key="index" class="list-group" ref="listGroup">
        <h2 class="list-group-title">{{group.title}}</h2>
        <ul>
          <li v-for="(item,index1) in group.items" v-bind:key="index1" class="list-group-item">
            <img class="avatar" v-lazy="item.avatar">
            <span class="name">{{item.name}}</span>
          </li>
        </ul>
      </li>
    </ul>
    <div class="list-shortcut" @touchstart.stop.prevent="onShortcutTouchStart"
         @touchmove.stop.prevent="onShortcutTouchMove">
      <ul>
        <li v-for="(item,index) in shortcutList" v-bind:key="index" class="item" :data-index="index"
            :class="{'current':currentIndex === index}">{{item}}
        </li>
      </ul>
    </div>
  </scroll>
</template>

<script type="text/ecmascript-6">
  import Scroll from '../scroll/scroll';
  import {getData} from '../../common/js/dom';

  const ANCHOR_HEIGHT = 18;

  export default {
    created() {
      this.touch = {};
      this.listHeight = [];
    },
    props: {
      data: {
        type: Array,
        default: function () {
          return [];
        }
      }
    },
    data: function () {
      return {
        currentIndex: 0,
        listenScroll: true,
        scrollY: -1,
        probeType: 3
      };
    },
    computed: {
      shortcutList() {
        return this.data.map((group) => {
          return group.title.substr(0, 1);
        });
      }
    },
    methods: {
      onShortcutTouchStart: function (e) {
        let anchorIndex = getData(e.target, 'index');
        this._scrollTo(anchorIndex);
        // move事件
        this.touch.y1 = e.touches[0].pageY;
        this.touch.anchorIndex = anchorIndex;
      },
      onShortcutTouchMove: function (e) {
        this.touch.y2 = e.touches[0].pageY;
        let delta = (this.touch.y2 - this.touch.y1) / ANCHOR_HEIGHT | 0;
        let anchorIndex = parseInt(this.touch.anchorIndex) + delta;

        this._scrollTo(anchorIndex);
      },
      _scrollTo: function (index) {
        if (!index && index !== 0) {
          return;
        }

        // onShortcutTouchMove滑动事件导致的index溢出
        if (index < 0) {
          index = 0;
        } else if (index > this.listHeight.length - 2) {
          index = this.listHeight.length - 2;
        }
        this.$refs.listview.scrollToElement(this.$refs.listGroup[index], 0);
        // 更新选择字母
        this.scrollY = -(this.listHeight[index]);
      },
      scroll: function (pos) {
        this.scrollY = pos.y;
      },
      _calculteHeight: function () {
        this.listHeight = [];
        const list = this.$refs.listGroup;
        let height = 0;
        this.listHeight.push(0);
        for (let i = 0; i < list.length; i++) {
          let item = list[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      }
    },
    watch: {
      data: function () {
        setTimeout(() => {
          this._calculteHeight();
        }, 20);
      },
      scrollY: function (newY) {
        const listHeight = this.listHeight;
        // 当滚动到顶部的时候
        if (newY > 0) {
          this.currentIndex = 0;
          return;
        }

        // 中间滚动
        for (let i = 0; i < listHeight.length - 1; i++) {
          let height1 = listHeight[i];
          let height2 = listHeight[i + 1];
          if (-newY >= height1 && -newY < height2) {
            this.currentIndex = i;
            return;
          }
        }

        // 底部滚动
        this.currentIndex = listHeight.length - 2;
      }
    },
    components: {
      Scroll
    }
  }
  ;
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl";
  .listview
    position: relative
    width: 100%
    height: 100%
    overflow: hidden
    background: $color-background
    .list-group
      padding-bottom: 30px
      .list-group-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
      .list-group-item
        display: flex
        align-items: center
        padding: 20px 0 0 30px
        .avatar
          width: 50px
          height: 50px
          border-radius: 50%
        .name
          margin-left: 20px
          color: $color-text-l
          font-size: $font-size-medium
    .list-shortcut
      position: absolute
      right: 0
      top: 50%
      transform: translateY(-50%)
      z-index: 30
      width: 20px
      padding: 20px 0
      border-radius: 10px
      background: $color-background-d
      text-align: center
      font-family: Helvetica
      .item
        padding: 3px
        line-height: 1
        color: $color-text-l
        font-size: $font-size-small
        &.current
          color: $color-theme
</style>
