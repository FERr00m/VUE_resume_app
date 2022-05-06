<template>
  <div class="form-control">
    <label :for="id">{{ label }}</label>
    <select
      :id="id"
      :value="modelValue"
      @change="onChange"
    >
      <option v-for="(option, i) in options" :key="i" :value="option.value">{{ option.name }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'AppSelect',
  emits: ['update:modelValue'],
  props: {
    modelValue: String,
    label: String,
    options: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      id: 'select-' + Math.random()
    }
  },
  methods: {
    onChange (e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>

<style scoped>

</style>
