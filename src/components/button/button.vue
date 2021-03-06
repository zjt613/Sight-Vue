<template>
  <button class="s-button" :class="classes" @click="emitClick" :disabled="disabled">
    <s-icon class="icon" v-if="icon && !loading" :name="icon"></s-icon>
    <s-icon class="icon loading" v-if="loading" name="loading"></s-icon>
    <div class="content">
      <slot></slot>
    </div>
  </button>
</template>

<script>
import Icon from '../icon/icon.vue'

export default {
  name: 'SightButton',
  components: {
    's-icon': Icon
  },
  props: {
    icon: String,
    loading: {
      type: Boolean,
      default: false
    },
    iconPosition: {
      type: String,
      default: 'left',
      validator (direction) {
        return ['left', 'right'].indexOf(direction) > -1
      }
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    classes () {
      return [`icon-${this.iconPosition}`, { 'disabled': this.disabled }]
    }
  },
  methods: {
    emitClick () {
      if (!this.loading && !this.disabled) {
        this.$emit('click')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  $font-size: 14px;
  $button-height: 32px;
  $button-bg: white;
  $button-active-bg: #eee;
  $border-radius: 4px;
  $color: #333;
  $border-color: #999;
  $border-color-hover: #666;

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }

  .s-button {
    font-size: $font-size;
    height: $button-height;
    padding: 0 1em;
    border-radius: $border-radius;
    border: 1px solid $border-color;
    background: $button-bg;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    vertical-align: middle;

    &:hover {
      border-color: $border-color-hover;
    }

    &:active {
      background-color: $button-active-bg;
    }

    &:focus {
      outline: none;
    }

    &.icon-left {
      > .icon {
        order: 1;
        margin-right: .3em;
      }

      > .content {
        order: 2;
      }
    }

    &.icon-right {
      > .icon {
        order: 2;
        margin-left: .3em;
      }

      > .content {
        order: 1;
      }
    }

    &.disabled {
      background-color: #999999;
    }

    .loading {
      animation: spin 1s infinite linear;
    }

    .content {
      padding: 0;
    }
  }
</style>
