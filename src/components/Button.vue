<script>
export default {
  name: "ButtonComponent",
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      console.log("click");
    },
  },
};
</script>

<template>
  <button
    @click="$emit('btn-click')"
    class="btn"
    :style="{ background: color }"
  >
    {{ text }}
  </button>
</template>

<style></style>
