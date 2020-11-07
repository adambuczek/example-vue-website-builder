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
    :clone="cloningMethod"
  >
    <div
      v-for="({ component }, i) in listItems"
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
import Draggable from 'vuedraggable'
import _cloneDeep from 'lodash/cloneDeep'

export default {
  name: 'ComponentsList',
  components: {
    Draggable
  },
  data() {
    return {
      listItems: []
    }
  },
  methods: {
    cloningMethod(original) {
      console.log(original)
      /**
       * deeply clone the data template
       */
      return {
        ...original,
        data: _cloneDeep(original.data)
      }
    }
  },
  async mounted () {
    const libraryUrl = 'http://localhost:8888/'
    const libraryManifest = await fetch(libraryUrl + 'manifest.json')
      .then(response => response.json())

    this.listItems = libraryManifest.components.map(component => {
      const componentWithSources = {
        ...component,
        img: libraryUrl + component.img,
        style: libraryUrl + component.style,
        script: libraryUrl + component.script
      }

      const data = component.props.reduce((accumulator, current) => {
        accumulator[current] = ''
        return accumulator
      }, {})

      return {
        component: componentWithSources,
        data
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