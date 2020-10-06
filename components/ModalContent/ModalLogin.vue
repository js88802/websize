<template>
  <AModal
    :centered="true"
    :closable="false"
    :footer="null"
    :width="568"
    :visible="modalVisible"
    :destroy-on-close="true"
    @cancel="closeModal()"
  >
    <ModalContentContainer :close-icon-event="closeModal">
      <FormGroup
        :error="emailForm.errorState"
      >
        <FormInputItem
          label="username"
          :value="emailForm.email"
          :set-value="setEmailForm('email')"
        />
        <FormInputItem
          label="password"
          :value="emailForm.password"
          :set-value="setEmailForm('password')"
        />
        <template v-slot:button>
          <FormButton style="margin-top: 20px;" />
        </template>
      </FormGroup>
    </ModalContentContainer>
  </AModal>
</template>

<script>
import ModalContentContainer from '@/components/ModalContent/ModalContentContainer.vue'
import FormGroup from '@/components/Form/FormGroup.vue'
import FormInputItem from '@/components/Form/FormInputItem.vue'
import FormButton from '@/components/Form/FormButton.vue'
export default {
  components: {
    ModalContentContainer,
    FormGroup,
    FormInputItem,
    FormButton
  },
  props: {
    visible: {
      type: Boolean,
      default () {
        return false
      }
    },
    closeEvent: {
      type: Function,
      default () {
        return null
      }
    }
  },
  data () {
    return {
      modalVisible: true,
      emailForm: {
        email: '',
        password: '',
        errorState: false
      }
    }
  },
  watch: {
    visible (newVal) {
      this.modalVisible = newVal
      if (!newVal) {
        this.resetEmailForm()
      }
    }
  },
  created () {
    this.modalVisible = this.visible
  },
  methods: {
    setEmailForm (type) {
      switch (type) {
        case 'email': return (value) => {
          this.emailForm.email = value
        }
        case 'password': return (value) => {
          this.emailForm.password = value
        }
        default: return null
      }
    },
    resetEmailForm () {
      this.emailForm = {
        email: '',
        password: '',
        errorState: false
      }
    },
    closeModal () {
      this.modalVisible = false
      this.closeEvent && this.closeEvent()
    }
  }
}
</script>

<style>

</style>
