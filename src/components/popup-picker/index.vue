<template>
  <popup :open="open">
    <div :class="classes">
      <div :class="['flexbox',cssPrefix + 'popup-picker-header']">
        <button type="button" :class="[cssPrefix + 'popup-picker-cancel']" @click="cancelHandler">{{cancelText}}</button>
        <button type="button" :class="['flexbox-item',cssPrefix + 'popup-picker-placeholder']">{{placeholder}}</button>
        <button type="button" :class="[cssPrefix + 'popup-picker-confirm']" @click="confirmHandler">{{confirmText}}</button>
      </div>
      <divider></divider>
      <div :class="[cssPrefix + 'popup-picker']">
        <picker
          v-if="open && myPickers"
          v-for="(item,index) in myPickers"
          :class="[cssPrefix + 'popup-picker-item']"
          :index="index"
          :value="item.value"
          :placeholder="item.placeholder"
          :options="item.options"
          @on-change="changeHandler"
        />
      </div>
    </div>
  </popup>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
import Popup from '../popup'
import Picker from '../picker'
import Divider from '../divider'

export default {
  components: {
    Popup,
    Picker,
    Divider
  },
  props: {
    open: {
      type: Boolean,
      default: false
    },
    pickers: {
      type: Array
    },
    placeholder: {
      type: String
    },
    cancelText: {
      type: String,
      default: '取消'
    },
    confirmText: {
      type: String,
      default: '完成'
    }
  },
  computed: {
    classes () {
      return [cssPrefix + 'popup-picker-wrapper']
    }
  },
  watch: {
    pickers (value) {
      this.myPickers = value
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      myPickers: this.pickers
    }
  },
  methods: {
    cancelHandler () {
      this.$emit('on-cancel')
    },
    confirmHandler () {
      let value = []
      for (let item of this.myPickers) {
        value.push({
          value: item.value
        })
      }
      this.$emit('on-confirm', value)
      this.$emit('input', value)
    },
    changeHandler (value, index) {
      this.myPickers[index].value = value
      this.$emit('on-change', value, index)
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  @import '~styles/mixins.scss';
  .#{$css-prefix}{
    &popup-picker{
      display:flex;
      &-wrapper{
        background:#fff;
      }
      &-item{
        flex:1;
        min-width:0;
      }
      &-header{
        display:flex;
      }
      &-placeholder{
        flex:1;
        line-height: 2.6rem;
        color:#999;
        min-width:0;
        overflow:hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
        @include button;
      }
      &-cancel,&-confirm{
        @include button;
        height: 2.6rem;
        width: 5rem;
        color:$sub-color;
      }
      &-confirm{
        color:$primary-color;
      }
    }
  }
</style>
