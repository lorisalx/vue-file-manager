<template>
  <div class="modal" v-if="visible">
    <div class="modal-background" @click="close"></div>
    <div class="modal-content">
      <div class="modal-header">
        <p>{{ fileName }}</p>
      </div>
      <VuePdfEmbed class="modal-pdf" :source="source" :width="800" :height="600"/>
    </div>
    <button class="modal-close is-large" @click="close"></button>
  </div>
</template>

<script>
import VuePdfEmbed from 'vue-pdf-embed/dist/vue2-pdf-embed'

export default {
  name: 'PdfModal',
  components: {
    VuePdfEmbed
  },
  props: {
    source: {
      type: String,
      required: true
    },
    visible: {
      type: Boolean,
      required: true
    },
    fileName: {
      type: String,
      required: true
    }
  },
  methods: {
    close() {
      this.$emit('close')
    }
  }
}
</script>

<style>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.modal-content {
  background: white;
  border-radius: 5px;
  max-width: 90%;
  max-height: 90%;
  overflow: auto;
}

.modal-pdf {
  padding: 20px;
}
.modal-close {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: none;
  font-size: 30px;
  cursor: pointer;
}

.modal-header {
  text-align: center;
  font-weight: bold;
  border-bottom: 1px solid #ccc;
}
</style>
