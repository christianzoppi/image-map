<template>
  <div>
    <div v-show="!modalIsOpen">
      <sb-asset-selector :uid="uid" field="image1"></sb-asset-selector>
      <!-- Preview -->
      <div class="sbi-image-map__preview-wrapper">
        <img :src="model.image" class="sbi-image-map__image-preview" />
        <ul class="sbi-image-map__nodes sbi-image-map__nodes">
          <li v-for="(node, index) in model.mapNodes" :key="index">
            <MapNode 
            :number=index 
            size="small"
            :coords=node></MapNode>
          </li>
        </ul>
      </div>
      <!-- Edit Button -->
      <SbButton @click=openModal class="sbi-image-map__button" size="small">Edit map</SbButton>
    </div>

    <!-- Modal -->
    <div v-if="modalIsOpen" @mouseup=stopDraggingNode>
      <!-- Controls -->
      <div class="sbi-image-map__controls">
        <SbButton size="small" icon="plus" class="sbi-image-map__close" @click=addNode>Add node</SbButton>
        <SbButton size="small" icon="close" class="sbi-image-map__close" has-icon-only @click=closeModal></SbButton>
      </div>
      <!-- Map -->
      <div class="sbi-image-map__image-wrapper" 
            @mousemove=dragNode>
        <img :src="model.image" class="sbi-image-map__image" ref="image" />
        <ul class="sbi-image-map__nodes">
          <li v-for="(node, index) in model.mapNodes" :key="index" @mousedown="() => startDraggingNode(index)">
            <MapNode 
            :number=index 
            @removeNode=removeNode
            :coords=node></MapNode>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import SbButton from 'storyblok-design-system/src/components/Button'
import MapNode from './MapNode.vue'

export default {
  mixins: [window.Storyblok.plugin],
  components: {
    SbButton,
    MapNode
  },
  data() {
    return {
      dragging: false,
      dragging_index: null,
      modalIsOpen: false
    }
  },
  methods: {
    initWith() {
      return {
        image: '',
        plugin: 'sbi-image-map',
        mapNodes: [
           
        ]
      }
    },
    openModal(){
      this.$emit('toggle-modal', true)
      this.modalIsOpen = true
    },
    closeModal(){
      this.$emit('toggle-modal', false)
      this.modalIsOpen = false
    },
    updateMapNode(elPosition, index) {
      const imageRect = this.$refs.image.getBoundingClientRect()
      const delta_x = elPosition.left - imageRect.left
      const delta_y = elPosition.top - imageRect.top
      const x_coord = delta_x >= 0 && delta_x <= imageRect.width ? delta_x / imageRect.width : this.model.mapNodes[index].x
      const y_coord = delta_y >= 0 && delta_y <= imageRect.height ? delta_y / imageRect.height : this.model.mapNodes[index].y
      this.model.mapNodes[index] = {x: x_coord, y: y_coord}
      this.model.mapNodes = this.model.mapNodes.slice()
    },
    startDraggingNode(index) {
      console.log('start dragging')
      this.dragging = true
      this.dragging_index = index
    },
    stopDraggingNode() {
      console.log('stop dragging')
      this.dragging = false
      this.dragging_index = null
    },
    dragNode(event) {
      if(this.dragging) {
        const imageRect = this.$refs.image.getBoundingClientRect()
        const delta_x = event.clientX - imageRect.left
        const delta_y = event.clientY - imageRect.top
        const x_coord = delta_x >= 0 && delta_x <= imageRect.width ? delta_x / imageRect.width : this.model.mapNodes[this.dragging_index].x
        const y_coord = delta_y >= 0 && delta_y <= imageRect.height ? delta_y / imageRect.height : this.model.mapNodes[this.dragging_index].y
        this.model.mapNodes[this.dragging_index] = {x: x_coord, y: y_coord}
        this.model.mapNodes = this.model.mapNodes.slice()
      }
    },
    addNode() {
      this.model.mapNodes = [...this.model.mapNodes, {x: 0.5, y: 0.5}] 
    },
    removeNode(index) {
      this.model.mapNodes.splice(index, 1)
    }
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  }
}
</script>

<style>
  .sbi-image-map__button {
    margin-top: 20px;
  }

  .sbi-image-map__preview-wrapper {
    margin-top: 20px;
    position: relative;
  }

  .sbi-image-map__preview {
    display: block;
    width: 100%;
  }

  .sbi-image-map__image-wrapper {
    border: #cacaca;
    position: relative;
    user-select: none;
  }

  .sbi-image-map__image-wrapper * {
    user-select: none;
  }

  .sbi-image-map__image {
    display: block;
  }

  .sbi-image-map__nodes {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  .sbi-image-map__controls {
    display: flex;
    justify-content: space-between;
    margin-bottom: 30px;
    width: 100%;
  }

  .sbi-image-map__close {
    display: block;
    margin: 0;
  }
  </style>
