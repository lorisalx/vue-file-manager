<template>
  <div
    v-if="item.type === 'folder'"
    @click="handleItemClick"
    class="item"
  >
    <div class="itemInfos folder">
      <FolderIcon class="icon"></FolderIcon>
      <span> {{ item.name }}</span>
    </div>
  </div>
  <div v-else class="item">
    <div class="itemInfos">
      <FileIcon class="icon"></FileIcon>
      <span> {{ item.name }}</span>
    </div>
    <div class="actions">
      <div @click="handleItemClick" >
        <EyeIcon class="actionItem" :class="{disabled: (item.extension !== 'pdf')}"></EyeIcon>
      </div>
      <DownloadIcon class="actionItem"></DownloadIcon>
    </div>
  </div>
</template>

<script>
import DownloadIcon from './icons/DownloadIcon.vue';
import FileIcon from './icons/FileIcon.vue';
import FolderIcon from './icons/FolderIcon.vue';
import EyeIcon from './icons/EyeIcon.vue'

export default {
  name: 'Item',
  components: {
    DownloadIcon,
    FileIcon,
    FolderIcon,
    EyeIcon
  },
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
<style scoped>
.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.actionItem:hover {
  cursor: pointer;
  box-shadow: rgba(0, 0, 0, 0.15) 0px 5px 15px;
  background-color: rgb(102, 177, 255, 0.2);
}

.disabled {
  opacity: 0.5;
  pointer-events: none;
}

.folder:hover {
  cursor: pointer;
  box-shadow: rgba(0, 0, 0, 0.15) 0px 5px 15px;
  background-color: rgb(102, 177, 255, 0.2);
}

.itemInfos {
  display: flex;
  flex-grow: 1;
  flex-direction: row;
  align-items: center;
  padding: 10px;
}
.icon {
  margin-right: 15px;
}

span {
  font-size: 16px;
  color: #333;
}

.actionItem {
  padding-left: 10px;
  padding-right: 10px;
  border-left: 1px solid #ccc;
  height: 100%;
  display: flex;
  align-items: center;
}

.actions {
  height: 100%;
  display: flex;
}

</style>
