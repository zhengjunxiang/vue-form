<template>
  <div>
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'iForm',
  data() {
    return { fields: [] }
  },
  props: {
    model: { type: Object },
    rules: { type: Object }
  },
  provide() {
    return { form: this }
  },
  methods: {
    resetFields() {
      this.fields.forEach(field => field.resetField())
    },
    validate(cb) {
      return new Promise(resolve => {
        let valid = true, count = 0;
        this.fields.forEach(field => {
          field.validate('', error => {
            if (error) valid = false;
            if (++count === this.fields.length) {
              resolve(valid);
              if (typeof cb === 'function') cb(valid);
            }
          })
        })
      })
    }
  },
  created() {
    this.$on('form-add', field => {
      if (field) this.fields.push(field);
    });
    this.$on('form-remove', field => {
      if (field.prop) this.fields.splice(this.fields.indexOf(field), 1);
    })
  }
}
</script>
