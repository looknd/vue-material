<template>
  <div class="component-tooltip"
    v-if="show"
    v-el:tooltip
    :style="{
      left: positionData.left +'px',
      top: positionData.top +'px'}"
    transition="tooltip">
    <span class="tooltip-text">
      {{text}}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      required: true
    },
    direction: {
      type: String,
      default: 'bottom'
    },
    target: {
      required: true
    },
    text: {
      type: String
    }
  },
  data () {
    return {
      positionData: {}
    }
  },
  computed: {
    tPosition () {
      return this.target.getBoundingClientRect()
    }
  },
  watch: {
    target (val) {
      this.$nextTick(() => {
        if (!this.$els.tooltip) return false
        this.positionData = this.computedPositon()
      })
    }
  },
  methods: {
    computedPositon () {
      let _tPosition = this.tPosition
      let _tWidth = _tPosition.width
      let _tHeight = _tPosition.height
      let _tLeft = _tPosition.left
      let _tTop = _tPosition.top

      let _margin = 14
      let domTooltip = this.$els.tooltip
      let _sWidth = domTooltip.offsetWidth
      let _sHeight = domTooltip.offsetHeight

      let setDirection = {
        top: () => {
          return {
            left: _tLeft + (_tWidth - _sWidth) / 2,
            top: _tTop - _margin - _sHeight
          }
        },

        right: () => {
          return {
            left: _tLeft + _margin + _tWidth,
            top: _tTop
          }
        },

        bottom: () => {
          return {
            left: _tLeft + (_tWidth - _sWidth) / 2,
            top: _tTop + _tHeight + _margin
          }
        },

        left: () => {
          return {
            left: _tLeft - _margin - _sWidth,
            top: _tTop
          }
        }
      }

      return setDirection[this.direction]()
    }
  }
}
</script>

<style lang="scss">
.component-tooltip {
  position: absolute;
  transition: transform .3s ease;

  .tooltip-text {
    font-size: 12px;
    overflow: hidden;
    padding: 6px 8px;
    opacity: .9;
    color: #fff;
    border-radius: 2px;
    background: #747474;
  }

  &.tooltip-enter,
  &.tooltip-leave {
    transform: perspective(1px) scale(1.1);
    opacity: 0;
  }
}
</style>
