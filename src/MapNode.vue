<template>
    <div :class="['map-node', size === 'small' ? 'map-node--small' : '', dragging ? 'map-node--dragging' : '' ]" :style=nodePosition()>{{ number + 1 }}
      <SbButton
        v-if="size !== 'small'"
        icon="close"
        is-rounded
        has-icon-only
        variant="ghost"
        @click=removeNode
      />
    </div>
</template>

<script>
import SbButton from 'storyblok-design-system/src/components/Button'

export default {
  components: {
    SbButton
  },
  props: ['coords', 'number', 'size', 'dragging'],
  methods: {
    nodePosition() {
      return `top: ${this.coords.y * 100}%; left: ${this.coords.x * 100}%`
    },
    removeNode() {
      this.$emit('removeNode', this.number)
    }
  }
}
</script>

<style>
  .map-node {
    align-items: center;
    background-color: #fd5fae;
    border-radius: 50%;
    color: #fff;
    display: flex;
    height: 30px;
    font-size: 1.2rem;
    font-weight: 700;
    justify-content: center;
    position: absolute;
    transform: translate(-50%, -50%);
    width: 30px;
  }

  .map-node:hover {
    cursor: grab;
  }

  .map-node--small {
    font-size: .3rem;
    height: 9px;
    width: 9px;
  }

  .map-node .sb-button {
    height: 15px;
    min-height: unset;
    padding: 0;
    position: absolute;
    right: -5px;
    top: -5px;
    width: 15px;
  }

  .map-node--dragging:hover {
    cursor: grabbing;
  }
</style>
