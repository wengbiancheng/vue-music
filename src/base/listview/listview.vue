<template>
  <scroll class="listview" ref="listview" :data="data">
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
    <div class="list-shortcut" @touchstart.stop.prevent="onShortcutTouchStart">
      <ul>
        <li v-for="(item,index) in shortcutList" v-bind:key="index" class="item" :data-index="index">{{item}}
        </li>
      </ul>
    </div>
  </scroll>
</template>

<script type="text/ecmascript-6">
  import Scroll from '../scroll/scroll';
  import {getData} from '../../common/js/dom';

  export default {
    props: {
      data: {
        type: Array,
        default: function () {
          return [];
        }
      }
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
      },
      _scrollTo(index) {
        if (!index && index !== 0) {
          return;
        }
        this.$refs.listview.scrollToElement(this.$refs.listGroup[index], 0);
      }
    },
    components: {
      Scroll
    }
  };
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
</style>
