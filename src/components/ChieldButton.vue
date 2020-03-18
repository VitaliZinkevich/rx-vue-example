<template>
  <div>
    <div>{{str}}</div>

    <button @click="strClicked">strClicked</button>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      str: ""
    };
  },
  methods: {
    strClicked() {
      this.str += ` clicked`;
      this.$emit("increment", { data: "xxx", array: [1, 2, 3] });
    }
  }
};
</script>