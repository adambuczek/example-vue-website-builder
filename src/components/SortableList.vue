<template>
  <Draggable
    :list="listItems"
    handle=".drag-handle"
    :class="{
      empty: !listItems.length,
      ul: true
    }"
    :group="{
      name: 'target',
      pull: false,
      put: ['source']
    }"
  >
    <div
      v-for="(item, i) in listItems"
      :key="i"
      class="item"
    >
      <span class="actions">
        <span class="close" @click="remove(i)">⨉</span>
        <span class="drag-handle">≡</span>
      </span>

      <ListItem :component="item" />

    </div>
  </Draggable>
</template>

<script>
import { VueDraggableNext } from 'vue-draggable-next'
import { createApp, h } from 'vue'

import loadExternalComponent from '../utils/loadExternalComponent.js'

const rm = (arr, i) => arr.slice(0, i).concat(arr.slice(i + 1))

const ListItem = {
  name: 'list-item',
  props: {
    component: Object,
    data: Object
  },
  render: () => h('div', { ref: 'host' }),
  mounted () {
    const host = this.$refs.host

    /**
     * @fix https://v3.vuejs.org/guide/migration/async-components.html#introduction
     */
    const component = () => loadExternalComponent(this.component.script)

    const render = () => h(component, {
      style: { border: '1px dotted midnightblue' },
      props: { ...this.data }
    })

    const shadowRoot = host.attachShadow({ mode: 'open' })
    const shadowApp = document.createElement('div')

    /** Load Styles */
    const shadowStyle = document.createElement('link')
    shadowStyle.href = this.component.style
    shadowStyle.rel = 'stylesheet'

    shadowRoot.appendChild(shadowApp)
    shadowRoot.appendChild(shadowStyle)

    createApp({ render }).mount(shadowApp)

  }
}

export default {
  name: 'SortableList',
  components: {
    Draggable: VueDraggableNext,
    ListItem
  },
  data() {
    return {
      listItems: [],
      dragover: false
    }
  },
  methods: {
    remove(i) {
      this.listItems = rm(this.listItems, i)
    }
  }
}
</script>

<style scoped>
.ul {
  list-style: none;
  padding: 0;
  margin: 0;
  min-height: 3em;
}

.ul.dragover {
    border: 1px solid red;
  }

.ul.empty {
  box-shadow: 0 0 10px inset rgba(0, 0, 0, .3);
  /* border: 1px solid red; */
}


.item:first-child {
  margin-top: 0;
}

.item {
  padding: 3em 1em 1em;
  margin: 1em 0;
  box-shadow: 0 0 1px rgba(0, 0, 0, .3);
  position: relative;
}

.actions {
  align-items: center;
  content: '';
  display: flex;
  font-size: 1em;
  font-weight: bolder;
  height: 3em;
  justify-content: center;
  line-height: 100%;
  position: absolute;
  right: 0;
  top: 0;
}

.actions span {
  margin-right: 1em;
}

.drag-handle {
  cursor: grab;
}

.close {
  cursor: pointer;
}
</style>