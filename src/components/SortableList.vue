<template>
  <Draggable
    :list="components"
    handle=".drag-handle"
    :class="{
      empty: !components.length,
      ul: true
    }"
    :group="{
      name: 'target',
      pull: false,
      put: ['source']
    }"
  >
    <div
      v-for="({ component, data }, i) in components"
      :key="i + component.name"
      class="item"
    >

      <span class="actions">
        <!-- <span class="edit" @click="edit(i)">🖉</span> -->
        <span class="close" @click="remove(i)">⨉</span>
        <span class="drag-handle">≡</span>
      </span>

      <div>
        <span
          v-for="prop in component.props"
          :key="prop"
        >
          <input
            type="text"
            v-model="data[prop]"
          />
        </span>
      </div>

      <ListItem
        :component="component"
        :data="data"
      />

    </div>
  </Draggable>
</template>

<script>
import Draggable from 'vuedraggable'
import Vue from 'vue'

import loadExternalComponent from '../utils/loadExternalComponent.js'

const rm = (arr, i) => arr.slice(0, i).concat(arr.slice(i + 1))

const ListItem = {
  name: 'list-item',
  props: {
    component: Object,
    data: Object
  },
  render: h => h('div', { ref: 'host' }),
  mounted () {
    const host = this.$refs.host

    const component = () => loadExternalComponent(this.component.script)

    const render = h => h(component, {
      style: { border: '1px dotted red' },
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

    new Vue({
      render
    }).$mount(shadowApp)
  }
}

export default {
  name: 'SortableList',
  components: {
    Draggable,
    ListItem
  },
  data() {
    return {
      components: [],
      componentsData: [],
      dragover: false
    }
  },
  methods: {
    remove(i) {
      this.components = rm(this.components, i)
      this.componentsData = rm(this.componentsData, i)
    },
    edit(event, index, prop) {
      const value = event.target.value
      const current = this.componentsData[index] || {}

      console.log(event, index, prop)

      Vue.set(this.componentsData, index, {
        ...current,
        [prop]: value
      })
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
.edit,
.close {
  cursor: pointer;
}
</style>