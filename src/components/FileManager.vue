<template>
  <div class="filemanager">
    <TopBar :path=currentPath @path-clicked="getPath" class="topbar"></TopBar>
    <div class="body">
      <Item
      v-for="(r, index) in items"
      :item="r"
      @item-clicked="getItems"
      :key="index"
      class="fileitem"
    />
    </div>

  </div>
</template>

<script>
import Item from './Item.vue'
import TopBar from './TopBar.vue'
import axios from 'axios';
export default {
  name: 'FileManager',
  components: {
    Item,
    TopBar
  },
  data() {
    return {
      currentPath: [],
      items: [
        { 'name': 'test', 'type': 'folder' },
        { 'name': 'test2', 'type': 'folder' },
        { 'name': 'test3', 'type': 'folder' },
        { 'name': 'filetest', 'type': 'file' }
      ]
    }
  },
  created() {
    this.getItems({ 'name': 'test', 'type': 'folder' });
  },
  methods: {
    async getItems(item) {
      if (item.type === 'folder') {
        
        try {
          this.currentPath.push(item.name);
          let path = encodeURIComponent(this.currentPath.join('/'));
          const response = await axios.get(`http://localhost:3000/api/files/${path}`);
          const folderContent = response.data;
          console.log(folderContent); 
          this.items = folderContent;
        } catch (error) {
          console.error('Error retrieving folder content:', error);
        }
      }
    },
    async getPath(index) {
      try {
          this.currentPath = this.currentPath.slice(0, index + 1)
          let path = encodeURIComponent(this.currentPath.join('/'));
          const response = await axios.get(`http://localhost:3000/api/files/${path}`);
          const folderContent = response.data;
          console.log(folderContent); 
          this.items = folderContent;
        } catch (error) {
          console.error('Error retrieving folder content:', error);
        }
    }
  }
}
</script>

<style>
.filemanager {
  width: 1000px;
  height: 600px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  border: 1px solid black;
  border-radius: 10px;
  box-shadow: rgb(204, 219, 232) 3px 3px 6px 0px inset, rgba(255, 255, 255, 0.5) -3px -3px 6px 1px inset;  background-color: rgb(243, 242, 242);
  padding: 10px;
}

.body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  width: 100%;
  height: 100%;
}

.fileitem {
  border: 1px solid black;
  background-color: white;
  width: 95%;
  margin: 5px;
  padding: 5px;
  transition: 0.3s;
}

.fileitem:hover {
  cursor: pointer;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 25px 50px -12px;
  background-color: rgb(211, 211, 211);
}

.topbar {
  margin-bottom: 20px;
}
</style>
