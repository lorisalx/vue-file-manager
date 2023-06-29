<template>
  <div class="modal" v-if="visible">
    <div class="modal-background" @click="close"></div>
    <div class="modal-content">
        <div class="modal-header">
          <p>{{ fileName }}</p>
        </div>
        <div class="modal-pdf">
          <div class="source" v-html="source"></div>
        </div>
    </div>
    <button class="modal-close is-large" @click="close"></button>
  </div>
</template>


<script>
export default {
  name: 'DocModal',
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
  },
  mounted() {
  const p = this.$refs.paragraph;
  if (p.getElementsByTagName('img').length > 0) {
    p.classList.add('has-img');
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
  position: relative;
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

.source img {
  max-width: 100%;
  align-items: center;
}

.has-img {
  text-align: center;
}
</style>
