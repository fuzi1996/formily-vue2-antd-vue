<template>
  <FormProvider :form="form">
    <ObjectField name="object">
      <template #default="{ field }">
        <div
          v-for="key in Object.keys(field.value || {})"
          :key="key"
          :style="{ marginBottom: '10px' }"
        >
          <Space>
            <Field :name="key" :component="[Input, { placeholder: key }]" />
            <Button @click="field.removeProperty(key)"> Remove </Button>
          </Space>
        </div>
        <Space>
          <Field
            name="propertyName"
            basePath=""
            required
            :component="[Input, { placeholder: 'Property Name' }]"
          />
          <Button @click="addPropertyToField(field)"> Add </Button>
        </Space>
      </template>
    </ObjectField>
  </FormProvider>
</template>

<script>
import { Input, Space, Button } from 'ant-design-vue'
import { createForm } from '@formily/core'
import { FormProvider, ObjectField, Field } from '@formily/vue'
import 'ant-design-vue/dist/antd.css'
// https://vue.formilyjs.org/api/components/object-field.html#%E7%94%A8%E4%BE%8B
export default {
  components: { FormProvider, ObjectField, Field, Space, Button },
  data() {
    return {
      Input,
      form: createForm(),
    }
  },
  methods: {
    addPropertyToField(field) {
      console.log('values',this.form.values)
      const name = this.form.values.propertyName
      if (name && !this.form.existValuesIn(`object.${name}`)) {
        field.addProperty(name, '')
        this.form.deleteValuesIn('propertyName')
      }
    },
  },
}
</script>