<template>
  <div class="filemanager">
    <TopBar :path=currentPath @path-clicked="getPath" class="topbar"></TopBar>
    <div class="divider"></div>
    <div class="body">
      <Item v-for="(r, index) in items" :item="r" @item-clicked="getItems" :key="index" class="fileitem" />
      <div v-if="pdf !== null">
        <PdfModal :fileName="modalFileName" :source="pdf" :visible="pdf !== null" @close="closePdfModal" />
      </div>
    </div>
  </div>
</template>

<script>
import Item from './Item.vue'
import TopBar from './TopBar.vue'
import axios from 'axios'
import PdfModal from './PdfModal.vue'

export default {
  name: 'FileManager',
  components: {
    Item,
    TopBar,
    PdfModal
  },
  data() {
    return {
      currentPath: [],
      items: [
        { 'name': 'test', 'type': 'folder' },
        { 'name': 'test2', 'type': 'folder' },
        { 'name': 'test3', 'type': 'folder' },
        { 'name': 'filetest', 'type': 'file' }
      ],
      pdf: null,
      modalFileName: null,
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
      } else if (item.type == 'file') {
          try {
            if (item.extension === 'pdf') {
              let pdfPath = [...this.currentPath];
              pdfPath.push(item.name);
              let path = encodeURIComponent(pdfPath.join('/'));
              const response = await axios.get(`http://localhost:3000/pdf/${path}`, { responseType: 'blob' });
              const url = window.URL.createObjectURL(new Blob([response.data]));
              this.pdf = url;            
              this.modalFileName = item.name;
            }
          }
          catch (error) {
            console.error('Error retrieving pdf content:', error);
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
    },
    closePdfModal() {
      this.pdf = null
    }
  }
}
</script>

<style>
.filemanager {
  width: auto;
  height: 50vh;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
  padding: 20px;
  background-color: rgb(255, 252, 252);
  border-radius: 15px;
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
  border: 1px solid #ccc;
  background-color: #ffffff;
  width: 95%;
  margin: 10px 0;
  transition: 0.3s;
  border-radius: 5px;
}

.topbar {
  width: 95%;
}

.divider {
  border-top: 1px solid #e0e0e0;
  width: 100%;
  opacity: 0.5;
  margin: 20px 0;
}
</style>
