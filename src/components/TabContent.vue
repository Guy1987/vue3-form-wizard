<template>
  <div
    v-show="active"
    v-if="!lazy || active"
    class="wizard-tab-container"
    role="tabpanel"
    :id="tabId"
    :aria-hidden="!active"
    :aria-labelledby="`step-${tabId}`"
  >
    <slot :active="active">
    </slot>
    <slot name="customIcon"></slot>
  </div>
</template>
<script>
  export default{
    name: 'tab-content',
    props: {
      title: {
        type: String,
        default: ''
      },
      /***
       * Icon name for the upper circle corresponding to the tab
       */
      icon: {
        type: String,
        default: ''
      },
      /***
       * Icon name for the upper circle corresponding to the tab
       */
      customIcon: {
        type: String,
        default: ''
      },
      /***
       * Only render the content when the tab is active
       */
      lazy: {
        type: Boolean,
        default: false
      },
      /***
       * Function to execute before tab switch. Return value must be boolean
       * If the return result is false, tab switch is restricted
       */
      beforeChange: {
        type: Function
      },
       /***
       * Function to execute after tab switch. Return void for now.
       * Safe to assume necessary validation has already occured
       */
      afterChange: {
        type: Function
      },
      route: {
        type: [String, Object]
      },
      additionalInfo: {
        type: Object,
        default: () => {}
      }
    },
    inject: ['addTab', 'removeTab'],
    data () {
      return {
        active: false,
        validationError: null,
        checked: false,
        tabId: ''
      }
    },
    computed: {
      shape () {
        return this.$parent.shape
      },
      color () {
        return this.$parent.color
      },
      errorColor () {
        return this.$parent.errorColor
      },
    },
    mounted () {
      this.addTab(this)
    },
    destroyed () {
      if (this.$el && this.$el.parentNode) {
        this.$el.parentNode.removeChild(this.$el)
      }
      this.removeTab(this)
    }
  }
</script>
