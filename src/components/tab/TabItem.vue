<template>
  <div class="tab" v-show="show">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    onShow: {
      type: Function,
      required: false
    }
  },
  data () {
    return {
      index: 0,
      show: false
    }
  },
  created () {
    this.index = this.$parent.items.length
    this.show = this.$parent.active === this.index
    this.$parent.items.push({
      title: this.title
    })
  },
  events: {
    'change': function (active) {
      this.show = this.$parent.active === this.index
    }
  },
  watch: {
    show (v, ov) {
      if (v === true && this.onShow) {
        this.onShow.call()
      }
    }
  },
  beforeDestroy () {
    this.$parent.items.splice(this.index, 1)
  }
}
</script>
