<template>
    <div :class="['map-node', size === 'small' ? 'map-node--small' : '' ]" :style=nodePosition()>{{ number + 1 }}
      <SbButton
        variant="ghost"
        icon="close"
        icon-description="Remove"
        is-rounded
        has-icon-only
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
  props: ['coords', 'number', 'size'],
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

  .map-node--small {
    font-size: .3rem;
    height: 9px;
    width: 9px;
  }

  .map-node .sb-button {
    height: 15px;
    padding: 0;
    position: absolute;
    right: -5px;
    top: -5px;
    width: 15px;
  }
</style>
