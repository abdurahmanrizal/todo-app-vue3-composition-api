<template>
  <div class="col-11">
    <input
      type="text"
      class="form-control"
      @input="$emit('update:modelValue', $event.target.value)"
      @keyup.enter="add"
      :value="modelValue" />
  </div>
</template>

<script>
export default {
  props: {
    modelValue: {
      type: String,
      default: "",
    },
  },
  setup(props, { emit }) {
    const add = () => {
      emit("add", props.modelValue);
    };
    return { add };
  },
};
</script>

<style></style>
