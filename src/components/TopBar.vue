<template>
<div>
  <span class="path" v-for="(r, index) in path" @click="handlePathClick(index)">
    {{ r }} <span v-if="index < path.length - 1">/</span>
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
<style>
.path:last-child{
  font-weight: bold;
}

.path:hover{
  cursor: pointer;
    text-decoration: underline;
}
</style>
