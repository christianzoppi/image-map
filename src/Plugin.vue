<template>
  <div>
    <div v-show="!modalIsOpen">
      Custom field is in sidebar / not in modal
      <button @click=openModal>Open</button>
    </div>
    <div v-if="modalIsOpen">
      Custom field is in not in sidebar but in a modal
      <button @click=closeModal>Close</button>
    </div>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      modalIsOpen: false
    }
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: 'sbi-image-map',
      }
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
    },
    openModal(){
      this.$emit('toggle-modal', true)
      this.modalIsOpen = true
    },
    closeModal(){
      this.$emit('toggle-modal', false)
      this.modalIsOpen = false
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
  .p-metatags__google-title {
    color: blue;
    text-decoration: underline;
  }

  .p-metatags__google-link {
    color: green;
  }

  .p-metatags__preview {
    margin: 5px 0 15px;
    padding: 10px;
    color: #000;
    background: #FFF;
  }
</style>
