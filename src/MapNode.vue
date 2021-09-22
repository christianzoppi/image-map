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
      <button @click="() => setDirection(item)" v-for="item in directions" :key="item" :class="['map-node__direction', `map-node__direction--${item}`, node.direction == item ? 'map-node__direction--active' : '']"></button>
    </div>
</template>

<script>
import SbButton from 'storyblok-design-system/src/components/Button'

export default {
  components: {
    SbButton
  },
  data: () => {
    return {
      directions: ['top', 'right', 'left', 'bottom', 'bottom-right', 'bottom-left', 'top-left', 'top-right']
    }
  },
  props: ['node', 'number', 'size', 'dragging'],
  methods: {
    nodePosition() {
      return `top: ${this.node.y * 100}%; left: ${this.node.x * 100}%`
    },
    removeNode() {
      this.$emit('removeNode', this.number)
    },
    setDirection(direction) {
      this.$emit('setDirection', {index: this.number, direction})
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

  .map-node__direction {
    background-color: rgb(143, 143, 143);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: block;
    height: 10px; 
    padding: 0;
    position: absolute;
    transform-origin: center;
    width: 10px; 
  }

  .map-node__direction--active {
    background-color: #fd5fae;
  }

  .map-node__direction--top {
    left: 50%;
    top: -14px;
    transform: translateX(-50%);
  }

  .map-node__direction--left {
    left: -18px;
    top: 50%;
    transform: translateY(-50%);
  }

  .map-node__direction--right {
    right: -18px;
    top: 50%;
    transform: translateY(-50%);
  }

  .map-node__direction--bottom {
    left: 50%;
    bottom: -14px;
    transform: translateX(-50%);
  }

  .map-node__direction--bottom-right {
    bottom: -7px;
    right: -15px;
    transform: translateX(-50%);
  }

  .map-node__direction--bottom-left {
    bottom: -7px;
    left: -4px;
    transform: translateX(-50%);
  }

  .map-node__direction--top-left {
    top: -7px;
    left: -4px;
    transform: translateX(-50%);
  }

  .map-node__direction--top-right {
    top: -7px;
    right: -15px;
    transform: translateX(-50%);
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
    right: -25px;
    top: -25px;
    width: 15px;
  }

  .map-node--dragging:hover {
    cursor: grabbing;
  }
</style>
