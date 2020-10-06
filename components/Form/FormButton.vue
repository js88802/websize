<template>
  <button class="form-button">
    <slot>
      按钮
    </slot>
  </button>
</template>

<script>
export default {
}
</script>

<style lang="less">
.form-button {
  width: 100%;
  height: 48px;
  font-size: 16px;
  font-weight: 600;
  color: black;
  background-color: transparent;
  border: 1px solid  #d9d9d9;
  border-radius: 8px;
  transition: 0.2s;

  &:hover {
    opacity: 0.8;
  }

  cursor: pointer;
  outline: none;
}
</style>
