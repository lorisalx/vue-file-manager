<template>
<div>
  <span class="path" v-for="(item, index) in path" @click="handlePathClick(index)">
    {{ item }}
  </span>
</div>
</template>

<script>

export default {
  name: 'TopBar',
  props: {
    path: Array
  },
  methods: {
    handlePathClick(item) {
      this.$emit('path-clicked', item);
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
  color: #409EFF;  
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
