<template>
  <div :class="classes" onselectstart="return false;">
    <slot></slot>
  </div>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
import { tab } from 'utils/mixins.js'
import Divider from '../divider'
export default {
  components: {
    Divider
  },
  mixins: [tab],
  computed: {
    classes () {
      return ['flexbox', cssPrefix + 'tabbar']
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      childLength: this.$children.length
    }
  },
  methods: {
    changeHandler (val) {
      this.$emit('click', val)
      if (val !== this.active) {
        this.$emit('on-change', val)
      }
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  @import '~styles/mixins.scss';
  .#{$css-prefix}{
    &tabbar{
      text-align:center;
      position:relative;
      user-select: none;
      background: #fff;
      &:before{
        @include divider;
        top:0;
      }
    }
  }
</style>
