<template>
<div>
  <span class="path" v-for="(r, index) in path" @click="handlePathClick(index)">
    {{ r }}
  </span>
</div>
</template>

<script>
const path = ["FileExplorer", "src", "components"];
export default {
  name: 'TopBar',
  props: {
    path: Array
  },
  methods: {
    handlePathClick(r) {
      this.$emit('path-clicked', r);
    }
  }
}
</script>
<style scoped>
.path {
  font-size: 16px;
  color: #333;
  margin: 0px;
  cursor: pointer;
  text-decoration: none;
  transition: color 0.3s;
}

.path:not(:last-child):hover {
  color: #007bff;
}

.path:not(:last-child)::after {
  content: ">";
  margin: 0 5px;
  color: #333;
}

.path:first-child {
  margin-left: 0;
}

.path:last-child {
  cursor: default;
  font-weight: bold;
}
</style>
