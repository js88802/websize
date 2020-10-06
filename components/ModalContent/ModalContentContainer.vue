<template>
  <div class="modal-content-container">
    <slot name="header">
      <header class="modal-content-header-container">
        <div class="left-container">
          <div class="close-icon-view" @click="closeIconClick()">
            <a-icon type="close" />
          </div>
        </div>
        <div class="center-container">
          <h1>{{ title }}</h1>
        </div>
      </header>
    </slot>
    <slot />
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default () {
        return 'default title'
      }
    },
    closeIconEvent: {
      type: Function,
      default () {
        return null
      }
    }
  },
  methods: {
    closeIconClick () {
      this.closeIconEvent && this.closeIconEvent()
    }
  }
}
</script>

<style lang="less">
.modal-content-container {
  width: 568px;
  background-color: white;
  border-radius: 12px;
}

.modal-content-header-container {
  width: 100%;
  height: 64px;
  border-bottom: 1px solid rgb(235, 235, 235);
  position: relative;

  & > .left-container {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    display: flex;
    align-items: center;
    padding-left: 20px;

    .close-icon-view {
      width: 32px;
      height: 32px;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      transition: 0.2s;
      border-radius: 50%;

      &:hover {
        background-color: rgb(247, 247, 247);
      }

      // icon config
      font-size: 16px;
      color: black;
    }
  }

  & > .center-container {
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    height: 100%;
    display: flex;
    align-items: center;

    h1 {
      color: rgb(34, 34, 34);
      font-size: 16px;
      padding: 0;
      margin: 0;
    }
  }
}
</style>
