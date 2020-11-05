<template>
  <Draggable
    :list="listItems"
    handle=".drag-handle"
    :group="{
      name: 'source',
      pull: 'clone',
      put: false
    }"
    :sort="false"
  >
    <div
      v-for="(component, i) in listItems"
      :key="i"
      class="item"
    >
      <span class="drag-handle">≡</span>
      <h1>{{ component.displayName }}</h1>
      <img :src="component.img" :alt="component.displayName">
    </div>
  </Draggable>
</template>

<script>
import { VueDraggableNext } from 'vue-draggable-next'

export default {
  name: 'ComponentsList',
  components: {
    Draggable: VueDraggableNext
  },
  data() {
    return {
      listItems: []
    }
  },
  async mounted () {
    const libraryUrl = 'http://localhost:8888/'
    const libraryManifest = await fetch(libraryUrl + 'manifest.json')
      .then(response => response.json())

    this.listItems = libraryManifest.components.map(component => {
      return {
        ...component,
        img: libraryUrl + component.img,
        style: libraryUrl + component.style,
        script: libraryUrl + component.script
      }
    })
  }
}
</script>

<style scoped>

.item:first-child {
  margin-top: 0;
}

.item {
  padding: 1em;
  margin: 1em 0;
  box-shadow: 0 0 1px rgba(0, 0, 0, .3);
  position: relative;
}

.item span {
  align-items: center;
  content: '';
  cursor: grab;
  display: flex;
  font-size: 1em;
  font-weight: bolder;
  height: 3em;
  justify-content: center;
  line-height: 100%;
  position: absolute;
  right: 0;
  top: 0;
  width: 3em;
}
</style>