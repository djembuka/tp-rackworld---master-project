<template>
  <div class="form-control">
    <select :name="name" @change="$emit('formcontrolchange', index)">
      <option
        v-for="(option, index) in options"
        :value="option.value"
        :key="index"
      >
        {{ option.text }}
      </option>
    </select>
    <hr />
  </div>
</template>

<script>
export default {
  name: 'FormControl',
  data() {
    return {};
  },
  props: ['index', 'name', 'options'],
  emits: ['formcontrolchange'],
};
</script>
