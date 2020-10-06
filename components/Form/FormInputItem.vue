<template>
  <div class="form-input-item">
    <label v-if="label" ref="label">
      {{ label }}
    </label>
    <AInput :value="value" v-bind="$attrs" @focus="inputFocusEvent()" @blur="inputBlurEvent()" @change="inputChangeEvent($event)" />
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default () {
        return null
      }
    },
    value: {
      type: String,
      default () {
        return ''
      }
    },
    setValue: {
      type: Function,
      default () {
        return null
      }
    }
  },
  methods: {
    inputFocusEvent () {
      this.labelStateToggle('focus')
    },
    inputBlurEvent () {
      this.labelStateToggle('blur')
    },
    inputChangeEvent (e) {
      const value = e.target.value
      this.setValue && this.setValue(value)
    },
    labelStateToggle (type = '') {
      const label = this.$refs.label || null
      if (!label) {
        return
      }
      switch (type) {
        case 'focus': return (label.classList.add('label-focus'))
        case 'blur':
          if (!this.value) {
            return (label.classList.remove('label-focus'))
          }
          return undefined
        default: return undefined
      }
    }
  }
}
</script>

<style lang="less">
.form-input-item {
  height: 57px;
  padding: 10px 12px;
  padding-top: 26px;
  border-top: 1px solid rgb(176, 176, 176);
  position: relative;

  &:first-child {
    border-top: none;
  }

  label {
    position: absolute;
    left: 0;
    top: 50%;
    transform-origin: top left;
    transform: translateY(-50%);
    height: 20px;
    color: rgb(113, 113, 113);
    font-size: 16px;
    padding-left: inherit;
    transition: 0.2s;
    user-select: none;
  }

  .label-focus {
    transform: translateY(-100%) scale(0.8);
  }

  .ant-input {
    padding: 0;
    margin: 0;
    outline: none;
    border: none;
    background-color: transparent;
    padding: inherit;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }

  .ant-input:hover {
    border: none !important;
  }

  .ant-input:focus {
    box-shadow: none;
    border: none !important;
    outline: none;
  }
}

.form-input-item-outline {
  outline: black;
}
</style>
