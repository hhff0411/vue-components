<template>
  <div :class="classes" @click="clickHandler">
    <i class="iconfont" v-html="icon"></i>
    <span>{{text}}</span>
  </div>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
export default {
  props: {
    text: {
      type: String
    },
    icon: {
      type: String
    }
  },
  methods: {
    clickHandler (e) {
      this.$emit('on-change', this.index)
      this.$emit('click', e)
    }
  },
  computed: {
    classes () {
      let array = ['flexbox-item', cssPrefix + 'tabbar-item']
      if (this.active) {
        array.push(cssPrefix + 'tabbar-item-active')
        array.push(this.$parent.activeClass)
      }
      return array
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      active: false
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  @import '~styles/mixins.scss';
  .#{$css-prefix}{
    &tabbar-item{
      padding:0.5rem 0;
      &-active{
        color:$primary-color;
        transition: color $transition-time $ease-in-out;
      }
      .iconfont{
        display:block;
      }
      span{
        font-size:0.7rem;
      }
    }
  }
</style>
