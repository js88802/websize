<template>
  <header v-if="isPC.state" ref="globalHeader" class="global-header-container">
    <div class="global-header-view">
      <div class="left-container">
        123
      </div>
      <div class="center-container">
        <div v-show="searchBoxType === 'little'" class="search-box-container">
          <!-- little searchbox -->
          <div class="search-box-view">
            <button class="btn" @click="littleSearchClick($event)">
              <div class="text-content">
                123
              </div>
              <div class="icon-container">
                <a-icon type="search" />
              </div>
            </button>
          </div>
        </div>
        <div v-show="searchBoxType === 'large'" class="options-box-container">
          <div class="options-box-header-view">
            <div
              class="type-item"
              :class="selectedType === 'Places to stay' ? 'type-item-selected' : null"
              @click="placeToStayClick()"
            >
              <span>Places to stay</span>
            </div>
            <div
              class="type-item"
              :class="selectedType === 'Experiences' ? 'type-item-selected' : null"
              @click="experiencesClick()"
            >
              <span>Experiences</span>
            </div>
          </div>
        </div>
      </div>
      <div class="right-container">
        <div class="chose-language-dropdown-container">
          <a-dropdown
            :trigger="['click']"
            :show-arrow="true"
            placement="bottomRight"
          >
            <div class="dropdown-view">
              <a-icon type="global" />
              <a-icon type="down" />
            </div>
            <a-menu slot="overlay" class="global-header-container-dropdown-menu">
              <a-menu-item key="0">
                <a href="http://www.alipay.com/">1st menu item</a>
              </a-menu-item>
              <a-menu-item key="1">
                <a href="http://www.taobao.com/">2nd menu item</a>
              </a-menu-item>
              <a-menu-item key="3">
                3rd menu item
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </div>
      </div>
    </div>
  </header>
  <header v-else-if="!isPC.state">
    <div>头部</div>
  </header>
</template>

<script>
class GlobalHeaderDOM {
  changeClassName (dom = new Element(), className = '') {
    dom.className = className
  }

  createModal ({ modalClickCallback = () => {}, windowScrollDownCallback = () => {} }) {
    if (window) {
      const modal = document.createElement('div')
      modal.className = 'global-header-container-modal'
      const mountedNode = document.body
      modal.onclick = () => {
        modalClickCallback && modalClickCallback()
        mountedNode.removeChild(modal)
      }
      mountedNode.appendChild(modal)

      let scrollEvent = null
      const scrollEventFn = (callback) => {
        return () => {
          const scrolledTop = window.scrollY
          if (scrolledTop >= 80) {
            callback && callback()
            mountedNode.removeChild(modal)
            window.removeEventListener('scroll', scrollEvent)
          }
        }
      }
      scrollEvent = scrollEventFn(windowScrollDownCallback)
      window.addEventListener('scroll', scrollEvent)
    }
  }
}

const globalHeaderDomMethods = new GlobalHeaderDOM()

export default {
  inject: {
    isPC: {
      // from: layout components
      type: Object,
      default () {
        return { state: true }
      }
    }
  },
  data () {
    return {
      searchBoxType: 'little',
      selectedType: 'Places to stay'
    }
  },
  methods: {
    littleSearchClick (e) {
      e.preventDefault()
      if (this.searchBoxType === 'large') {
        return
      }
      this.searchBoxType = 'large'
      const globalHeader = this.$refs.globalHeader
      globalHeaderDomMethods.changeClassName(globalHeader, 'global-header-container-search-focus')
      globalHeaderDomMethods.createModal({
        modalClickCallback: () => {
          globalHeaderDomMethods.changeClassName(globalHeader, 'global-header-container')
          this.searchBoxType = 'little'
        },
        windowScrollDownCallback: () => {
          globalHeaderDomMethods.changeClassName(globalHeader, 'global-header-container')
          this.searchBoxType = 'little'
        }
      })
    },
    placeToStayClick () {
      this.selectedType = 'Places to stay'
    },
    experiencesClick () {
      this.selectedType = 'Experiences'
    }
  }
}
</script>

<style lang="less" scoped>
.global-header-container {
  height: 80px !important;
  position: fixed !important;
  top: 0;
  left: 0px !important;
  width: 100% !important;
  z-index: 100 !important;
  --header-brand-color: #FF385C !important;
}

.global-header-container-search-focus {
  height: 80px !important;
  position: fixed !important;
  left: 0px !important;
  top: 0;
  width: 100% !important;
  z-index: 100 !important;
  --header-brand-color: #FF385C !important;

  &::before {
    background-image: linear-gradient(rgb(0, 0, 0), rgba(0, 0, 0, 0)) !important;
    content: "" !important;
    height: 140% !important;
    left: 0px !important;
    opacity: 0 !important;
    pointer-events: none !important;
    position: absolute !important;
    top: 0px !important;
    transition: opacity 150ms ease 0s !important;
    width: 100% !important;
    z-index: 0 !important;
  }

  &::after {
    box-shadow: rgba(0, 0, 0, 0.08) 0px 1px 12px !important;
    content: "" !important;
    height: 100% !important;
    left: 0px !important;
    position: absolute !important;
    top: 0px !important;
    width: 100% !important;
    z-index: 0 !important;
    transform-origin: 50% 0% !important;
    opacity: 1 !important;
    transform: scaleY(3.05) !important;
    background: rgb(255, 255, 255) !important;
    transition: opacity 150ms ease 0s, -ms-transform, -webkit-transform, transform !important;
  }
}

.global-header-container::before {
  background-image: -webkit-linear-gradient(to bottom, #000, rgba(0, 0, 0, 0)) !important;
  background-image: -moz-linear-gradient(to bottom, #000, rgba(0, 0, 0, 0)) !important;
  background-image: linear-gradient(to bottom, #000, rgba(0, 0, 0, 0)) !important;
  content: "" !important;
  height: 140% !important;
  left: 0px !important;
  opacity: 0 !important;
  pointer-events: none !important;
  position: absolute !important;
  top: 0px !important;
  -webkit-transition: 150ms opacity ease !important;
  -moz-transition: 150ms opacity ease !important;
  transition: 150ms opacity ease !important;
  width: 100% !important;
  z-index: 0 !important;
}

.global-header-container::after {
  background: #FFFFFF !important;
  box-shadow: rgba(0, 0, 0, 0.08) 0px 1px 12px !important;
  content: "" !important;
  height: 100% !important;
  left: 0px !important;
  position: absolute !important;
  top: 0px !important;
  -webkit-transition: opacity 0.3s cubic-bezier(0.35, 0, 0.65, 1) !important;
  -moz-transition: opacity 0.3s cubic-bezier(0.35, 0, 0.65, 1) !important;
  transition: opacity 0.3s cubic-bezier(0.35, 0, 0.65, 1) !important;
  width: 100% !important;
  z-index: 0 !important;
  -webkit-transform-origin: 50% 0% !important;
  -ms-transform-origin: 50% 0% !important;
  transform-origin: 50% 0% !important;
  -webkit-transition-duration: 150ms !important;
  transition-duration: 150ms !important;
  -webkit-transition-property: opacity, -webkit-transform, transform !important;
  -moz-transition-property: opacity, transform !important;
  transition-property: opacity, -ms-transform, -webkit-transform, transform !important;
  -webkit-transition-timing-function: ease !important;
  transition-timing-function: ease !important;
  opacity: 1 !important;
}

.global-header-view {
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  -webkit-box-align: center;
  -ms-flex-align: center;
  -webkit-align-items: center;
  align-items: center;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  height: 100%;
  position: relative;
  width: 100%;
  z-index: 1;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  padding-left: 24px;
  padding-right: 24px;
  margin: 0 auto;
}

.left-container {
  -webkit-flex: 0 0 auto !important;
  -ms-flex: 0 0 auto !important;
  flex: 0 0 auto !important;
  height: 100%;
  justify-content: flex-start;
  align-items: center;
}

.right-container {
  -webkit-flex: 1 0 auto !important;
  -ms-flex: 1 0 auto !important;
  flex: 1 0 auto !important;
  height: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;

  .chose-language-dropdown-container {
    width: 55px;
    height: 42px;
    line-height: 42px;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    & > .dropdown-view {
      width: 100%;
      height: 100%;
      border-radius: 22px;
      transition: 0.2s;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 10px;

      &:hover {
        background-color: rgb(247, 247, 247);
      }
    }
  }
}

.center-container {
  -webkit-flex: 0 1 auto !important;
  -ms-flex: 0 1 auto !important;
  flex: 0 1 auto !important;
  min-width: 0px !important;
  padding: 0 24px !important;
  text-align: left !important;
  width: 348px;
  height: 48px;
  position: relative;
}

.search-box-container {
  display: inline-block;
  vertical-align: middle !important;
  text-align: left !important;
  -webkit-transform-origin: 0% 0% !important;
  -ms-transform-origin: 0% 0% !important;
  transform-origin: 0% 0% !important;
  -webkit-transition: -webkit-transform 150ms ease,transform 150ms ease, opacity 50ms ease 20ms, pointer-events 0ms 150ms !important;
  -moz-transition: transform 150ms ease, opacity 50ms ease 20ms, pointer-events 0ms 150ms !important;
  transition: -ms-transform 150ms ease,-webkit-transform 150ms ease,transform 150ms ease, opacity 50ms ease 20ms, pointer-events 0ms 150ms !important;
}

.search-box-view {
  -webkit-box-align: center !important;
  -ms-flex-align: center !important;
  -webkit-align-items: center !important;
  align-items: center !important;
  background-color: #FFFFFF !important;
  border: 1px solid #DDDDDD !important;
  border-radius: 24px !important;
  color: #222222 !important;
  display: -webkit-inline-box !important;
  display: -moz-inline-box !important;
  display: -ms-inline-flexbox !important;
  display: -webkit-inline-flex !important;
  display: inline-flex !important;
  max-width: 100% !important;
  text-align: left !important;
  -webkit-transition: box-shadow 0.2s ease !important;
  -moz-transition: box-shadow 0.2s ease !important;
  transition: box-shadow 0.2s ease !important;
  vertical-align: middle !important;

  &:hover {
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.18) !important;
  }

  & > .btn {
    -webkit-box-align: center !important;
    -ms-flex-align: center !important;
    -webkit-appearance: none !important;
    -moz-appearance: none !important;
    appearance: none !important;
    background: transparent !important;
    border: 1px solid transparent !important;
    color: inherit !important;
    cursor: pointer !important;
    display: -webkit-box !important;
    display: -moz-box !important;
    display: -ms-flexbox !important;
    display: -webkit-flex !important;
    display: flex !important;
    font-family: inherit !important;
    font-size: inherit !important;
    font-weight: inherit !important;
    line-height: inherit !important;
    margin: -1px !important;
    outline: none !important;
    overflow: visible !important;
    padding: 0px !important;
    text-align: inherit !important;
    text-decoration: none !important;
    -webkit-user-select: auto !important;
    -moz-user-select: auto !important;
    -ms-user-select: auto !important;
    user-select: auto !important;
    -webkit-align-items: center !important;
    align-items: center !important;
    border-radius: 4px !important;
    -webkit-flex: 0 1 auto !important;
    -ms-flex: 0 1 auto !important;
    flex: 0 1 auto !important;
    height: 48px !important;
    min-width: 0px !important;
    position: relative !important;
    z-index: 1 !important;

    &:first-of-type {
      padding-left: 8px !important;
      border-top-left-radius: inherit !important;
      border-bottom-left-radius: inherit !important;
    }

    &:last-of-type {
      border-top-right-radius: inherit !important;
      border-bottom-right-radius: inherit !important;
    }

    &:only-of-type {
      width: 300px !important;
    }

    .text-content {
      font-size: 14px !important;
      line-height: 18px !important;
      -webkit-flex: 1 1 auto !important;
      -ms-flex: 1 1 auto !important;
      flex: 1 1 auto !important;
      min-width: 0px !important;
      font-weight: 600 !important;
      padding: 0 16px !important;
      text-overflow: ellipsis !important;
      white-space: nowrap !important;
      overflow: hidden !important;
    }

    .icon-container {
      background-color: #FF385C !important;
      border-radius: 50% !important;
      color: #FFFFFF !important;
      -webkit-flex: 0 0 32 !important;
      -ms-flex: 0 0 32 !important;
      flex: 0 0 32 !important;
      height: 32px !important;
      margin: 7px 7px 7px 0 !important;
      padding: 10px !important;
      width: 32px !important;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 12px;
    }
  }
}

.options-box-container {
  display: flex;
  justify-content: center;
  padding-left: 24px !important;
  padding-right: 24px !important;
  left: 0px !important;
  position: absolute !important;
  text-align: left !important;
  top: 64px !important;
  transform-origin: 60px 0% !important;
  max-width: 850px !important;
  width: auto !important;
  z-index: 2 !important;
  transition: -ms-transform 150ms ease 0s, -webkit-transform 150ms ease 0s, transform 150ms ease 0s, opacity 50ms ease 20ms, pointer-events 0ms ease 150ms !important;
}

.options-box-header-view {
  height: 48px;
  display: flex;
  align-items: center;

  .type-item {
    white-space: nowrap;
    font-size: 16px;
    color: #222222;
    padding: 10px 12px;
    cursor: pointer;
    position: relative;

    &::after {
      content: '';
      position: absolute;
      left: 50%;
      bottom: 0;
      background-color: #222222;
      height: 2px;
      width: 18px;
      transition: 0.2s;
      transform: translateX(-50%) scale(0, 1);
    }

    &:hover {
      opacity: 0.8;

      &::after {
        transform: translateX(-50%) scale(0.3, 1);
      }
    }
  }

  .type-item-selected {
    cursor: default;
    &:hover {
      opacity: 1;

      &::after {
        transform: translateX(-50%) scale(1, 1);
      }
    }
    &::after {
      transform: translateX(-50%) scale(1, 1);
    }
  }
}

@media (min-width: 375px) {
  .global-header-view {
    padding-left: 24px !important;
    padding-right: 24px !important;
  }

  .options-box-container {
    padding-left: 24px !important;
    padding-right: 24px !important;
  }
}

@media (min-width: 744px) {
  .global-header-view {
    padding-left: 40px !important;
    padding-right: 40px !important;
  }

  .options-box-container {
    position: fixed !important;
    bottom: 0 !important;
    left: 50% !important;
    height: 0 !important;
    transform: translateX(-50%);
    padding-left: 40px !important;
    padding-right: 40px !important;
  }
}

@media (min-width: 950px) {
  .global-header-container-search-focus::after {
    transform: scaleY(2.25) !important;
  }

  .global-header-view {
    padding-left: 40px !important;
    padding-right: 40px !important;
  }

  .center-container {
    text-align: center !important;
  }

  .left-container {
    -webkit-flex: 1 0 140px !important;
    -ms-flex: 1 0 140px !important;
    flex: 1 0 140px !important;
  }

  .right-container {
    -webkit-flex: 1 0 140px !important;
    -ms-flex: 1 0 140px !important;
    flex: 1 0 140px !important;
  }

  .options-box-container {
    top: 0px !important;
    transform-origin: 50% 0% !important;
    margin-top: 16px !important;
  }
}

@media (min-width: 1128px) {
  .global-header-view {
      padding-left: 80px !important;
      padding-right: 80px !important;
  }
}

@media (min-width: 1440px) {
  .global-header-view {
    padding-left: 80px !important;
    padding-right: 80px !important;
  }
}
</style>

<style lang="less">
.global-header-container-modal {
  z-index: 99;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: pink;
}

.global-header-container-dropdown-menu {
  border-radius: 12px;
  .ant-dropdown-menu-item {
    &:hover {
      background-color: rgb(247, 247, 247);
    }
  }
}
</style>
