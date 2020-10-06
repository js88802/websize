<template>
  <div class="form-group">
    <div class="form-group-input-view">
      <slot />
    </div>
    <div v-if="error" class="form-group-error">
      <slot name="error" />
    </div>
    <slot name="button" />
  </div>
</template>

<script>
export default {
  props: {
    error: {
      type: Boolean,
      default () {
        return false
      }
    }
  }
}
</script>

<style lang="less">
.form-group {
  padding: 20px;
  .form-group-input-view {
    border: 1px solid rgb(176, 176, 176);
    border-radius: 8px;
  }

  .form-group-error {
    font-size: 12px;
    color: rgb(193, 53, 21);
  }
}
</style>
