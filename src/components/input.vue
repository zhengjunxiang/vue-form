<template>
  <div>
    <input ref="input" :type="type" :value="currentValue" @input="handleInput" @blur="handleBlur" />
  </div>
</template>
<script>
import Emitter from '@/mixins/emitter.js';
export default {
  name: 'iInput',
  mixins: [ Emitter ],
  props: {
    type: { type: String, default: 'text'},
    value: { type: String, default: ''}
  },
  watch: {
    value(value) {
      this.currentValue = value
    }
  },
  data() {
    return { currentValue: this.value, id: this.label }
  },
  mounted () {
    if (this.$parent.labelFor) this.$refs.input.id = this.$parent.labelFor;
  },
  methods: {
    handleInput(e) {
      const value = e.target.value;
      this.currentValue = value;
      this.$emit('input', value);
      this.dispatch('iFormItem', 'form-change', value);
    },
    handleBlur() {
      this.dispatch('iFormItem', 'form-blur', this.currentValue);
    }
  }
}
</script>