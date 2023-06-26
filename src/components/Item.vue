<template>
  <div
    v-if="item.type === 'folder'"
    @click="handleItemClick"
  >
    <img alt="Folder" src="../assets/folder.png" width="10px" height="10px"/>
    <span> {{ item.name }}</span>
  </div>
  <div v-else>
    <img alt="File" src="../assets/file.png" width="10px" height="10px"/>
    <span> {{ item.name }}</span>
  </div>
</template>

<script>
export default {
  name: 'Item',
  props: {
    item: Object
  },
  methods: {
    handleItemClick() {
      this.$emit('item-clicked', this.item);
    }
  }
}
</script>

<style>

</style>
