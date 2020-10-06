<template>
  <div style="background-color: black; height: 2000px;">
    <ModalLogin :visible="modalLoginVisibleState" :close-event="setModalLoginVisibleState('hidden')" />
    <button @click="buttonClick()">
      asd
    </button>
  </div>
</template>

<script>
import ModalLogin from '@/components/ModalContent/ModalLogin.vue'
export default {
  components: {
    ModalLogin
  },
  data () {
    return {
      modalLoginVisibleState: false
    }
  },
  methods: {
    // modalLoginEvent
    setModalLoginVisibleState (type) {
      switch (type) {
        case 'visible': return () => {
          this.modalLoginVisibleState = true
        }
        case 'hidden': return () => {
          this.modalLoginVisibleState = false
        }
        default: return null
      }
    },
    // btn click
    buttonClick () {
      const openModalLogin = this.setModalLoginVisibleState('visible')
      openModalLogin()
    }
  }
}
</script>
