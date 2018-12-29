<template>
  <div class="home">
    <h3>Form (校验表单)</h3>
    <iForm ref="form" :model="formData" :rules="rules">
      <iFormItem label="名称：" prop="name">
        <iInput v-model="formData.name"></iInput>
      </iFormItem>
      <iFormItem label="邮箱：" prop="mail">
        <iInput v-model="formData.mail"></iInput>
      </iFormItem>
      <button @click="handleSubmit">提交</button>
      <button @click="handleReset">重置</button>
    </iForm>
  </div>
</template>

<script>
// @ is an alias to /src
import iForm from '@/components/form/form.vue'
import iFormItem from '@/components/form/formItem.vue'
import iInput from '@/components/input.vue'

export default {
  name: 'home',
  components: { iForm, iFormItem, iInput },
  data() {
    return {
      formData: { name: '', mail: '' },
      rules: {
        name: [{ required: true, message: '不能为空', trigger: 'blur'}],
        mail: [
          { required: true, message: '不能为空', trigger: 'blur'},
          { type: 'email', message: '邮箱格式不正确', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    handleSubmit() {
      this.$refs.form.validate((valid) => {
        if (valid)  console.log('提交成功');
        else console.log('校验失败');
      })
    },
    handleReset() { this.$refs.form.resetFields() }
  }
}
</script>
