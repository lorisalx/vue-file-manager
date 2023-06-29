<template>
  <div class="filemanager">
      <div class="navigation">
        <TopBar :path=currentPath @path-clicked="getPath" class="topbar"></TopBar>
      </div>

      <div class="body">
        <Item v-for="(r, index) in items" :item="r" @item-clicked="getItems" :key="index" class="fileitem" />
        <div v-if="pdf !== null">
          <PdfModal :fileName="modalFileName" :source="pdf" :visible="pdf !== null" @close="closePdfModal" />
        </div>
        <DocModal :fileName="modalFileName" :source="docHtml" :visible="docHtml !== null" @close="closeDocModal" />
      </div>
  </div>
</template>

<script>
import Item from './Item.vue'
import TopBar from './TopBar.vue'
import axios from 'axios'
import PdfModal from './PdfModal.vue'
import DocModal from './DocModal.vue'
export default {
  name: 'FileManager',
  components: {
    Item,
    TopBar,
    PdfModal,
    DocModal
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
      docHtml: null,

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
            if (item.extension === 'docx') {
              let docPath = [...this.currentPath];
              docPath.push(item.name);
              let path = encodeURIComponent(docPath.join('/'));

              axios.get(`http://localhost:3000/api/filees/${path}`)
                .then(response => {
                  this.docHtml = response.data;
                  this.modalFileName = item.name;
                  console.log(this.modalFileName);
                  console.log(this.docHtml);
                })
                .catch(err => {
                  console.error(err);
                });
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
    },
    closeDocModal () {
      this.docHtml = null
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
  background-color: rgb(255, 252, 252);
  border-radius: 15px;
  overflow: auto;
  position: relative;
}

.filemanager * {
  box-sizing: border-box;
}

.navigation {
  position: sticky;
  top: 0;
  z-index: 10;
  background: rgb(255, 252, 252);
  width: 100%;
  padding: 20px 20px 10px 20px;
  border-bottom: 1px solid #e0e0e0;

}

.body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  width: 100%;
  padding: 20px;

}

.fileitem {
  border: 1px solid #ccc;
  background-color: #ffffff;
  width: 100%;
  margin: 10px 0;
  transition: 0.3s;
  border-radius: 5px;
}

.topbar {
  width: 95%;
}

</style>